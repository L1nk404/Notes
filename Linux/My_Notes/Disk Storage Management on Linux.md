Reference guide with commands and tools for investigating and freeing up disk space via the terminal.

## Table of Contents
- [[#Quick diagnosis]]
- [[#du - Disk usage by folder]]
- [[#df - Disk usage by partition]]
- [[#ncdu - Visual space browser]]
- [[#Btrfs - Specific commands]]
- [[#Snapper - Snapshot management]]
- [[#Common pitfalls]]
- [[#General system cleanup]]
- [[#Investigation checklist]]

---

## Quick diagnosis

Recommended order when the disk is full:
1. `df -h` → confirm which partition is full
2. `du`/`ncdu` → try to find large folders
3. If the system is **Btrfs** → don't blindly trust `du`/`ncdu` (see dedicated section)
4. Check journal, package cache, containers/VMs
5. Check for deleted files still held open by processes

---

## `du` - Disk usage by folder

```bash
# Total size of a specific folder
du -sh /path/to/folder

# Size of each first-level subfolder
du -h --max-depth=1 /path

# Sorted from largest to smallest
du -h --max-depth=1 /path | sort -rh

# Don't cross into other filesystems/mount points (avoids counting snaps, /proc, etc.)
du -h -x --max-depth=1 /
```

> [!warning] 
> Be careful with `-x` on systems with bind mounts If `/home`, `/var`, `/opt`, etc. are **separate mount points** from the same partition (common in setups with subvolumes), `-x` will _skip_ those folders and mask where the actual usage is. In that case, run `du` **without `-x`**, folder by folder, specifically on those mount points.

---

## `df` - Disk usage by partition

```bash
# Overview, human-readable format
df -h

# See filesystem type (ext4, xfs, btrfs...)
df -hT

# See inodes (when "no space" but du/df show free space)
df -i
```

|Column|Meaning|
|---|---|
|Size|Total size|
|Used|Space used|
|Avail|Space available|
|Use%|Percentage used|
|Mounted on|Mount point|

> [!tip] 
> `du` vs `df` `du` measures folders/files. `df` measures the entire partition/disk. When the two **don't match** (df shows much more used than the sum from du), it's a sign of: a deleted file still open by a process, or a filesystem quirk (e.g., Btrfs with snapshots).

---

## `ncdu` - Visual space browser

Interactive text-mode interface, navigate with arrow keys — much faster than running `du` repeatedly.

```bash
# Install
sudo apt install ncdu        # Debian/Ubuntu
sudo zypper install ncdu     # openSUSE

# Run (always with sudo to see everything correctly)
sudo ncdu -x /
```

> [!warning] 
> Running without `sudo` masks results System folders without read permission show up with an error (`!` or `e` in the interface) and are undercounted or ignored — always run with `sudo` for a reliable diagnosis.

> [!danger] 
> ncdu and Btrfs don't mix well On Btrfs systems with snapshots, `ncdu` counts the **apparent** size of each file in each snapshot, without knowing that multiple snapshots share the same physical blocks (CoW). This can make `/.snapshots` appear to be **terabytes**, even though the entire disk has only a few GB of real space actually occupied by them. See the Btrfs section below.

---

## Btrfs - Specific commands

Systems with Btrfs (default on openSUSE, common on Fedora) use **Copy-on-Write (CoW)**: snapshots don't duplicate data, they just reference the same physical blocks. This breaks the logic of traditional tools like `du`.

### View actual filesystem usage

```bash
sudo btrfs filesystem usage /
```

Important fields:

- **Device allocated**: space that btrfs has reserved (can be larger than what's used)
- **Used**: space actually occupied by data
- **Unallocated**: free space not yet reserved

### View exclusive vs. shared size of snapshots

```bash
sudo btrfs filesystem du -s /.snapshots/*/snapshot
```

- **Total**: apparent size (includes blocks shared with other snapshots)
- **Exclusive**: space that would **actually be freed** if that snapshot were deleted
- **Set shared**: blocks shared with other snapshots/subvolumes

> [!tip] 
> What actually matters To decide what to delete, look at the **Exclusive** column, not Total. A snapshot might show "11GB total" but have "0B exclusive" — meaning deleting it frees nothing, because the data still exists in other snapshots.

### View all subvolumes (mounted or not)

```bash
sudo btrfs subvolume list -a /
```

Useful for finding "orphan" subvolumes created by Docker, libvirt, Distrobox, etc., which don't show up in the normal folder tree.

### View subvolumes marked for deletion (pending cleanup)

```bash
sudo btrfs subvolume list -d /
```

If this comes back empty, there's no pending cleanup. If entries show up, the kernel is still processing the deletion of snapshots in the background — this can take minutes to hours.

### Qgroups (detailed exclusive usage, requires quotas enabled)

```bash
sudo btrfs quota enable /
sudo btrfs qgroup show -pc / | sort -k3 -rh | head -20
```

### Balance (consolidate empty blocks and free allocated space)

```bash
# Rebalances only 100% empty data blocks (safe and fast)
sudo btrfs balance start -dusage=0 /

# See progress
sudo btrfs balance status /

# Cancel if needed
sudo btrfs balance cancel /
```

> [!note] 
> Why the space doesn't show up right away Deleting a snapshot (`snapper delete`) doesn't free space instantly. The kernel processes the block removal in the background (**cleaner thread**). On systems with many accumulated snapshots (months of updates), this can take a while. Running `sync` or restarting the system usually forces it to finish.

---

## Snapper - Snapshot management

Standard tool on openSUSE (and available on other distros) for managing Btrfs snapshots, usually created automatically before/after each update (`zypper`).

```bash
# List all snapshots
sudo snapper list

# Delete a specific snapshot
sudo snapper delete <number>

# Delete a range
sudo snapper delete <start>-<end>

# View existing configurations
sudo snapper list-configs
```

> [!warning] 
> Snapshot 0 (current) cannot be deleted Snapshot `0` represents the currently running system — it cannot be deleted.

### Configure automatic retention (prevent future buildup)

Edit `/etc/snapper/configs/root` and adjust:

```
NUMBER_LIMIT="10"
NUMBER_LIMIT_IMPORTANT="10"
TIMELINE_LIMIT_HOURLY="5"
TIMELINE_LIMIT_DAILY="7"
TIMELINE_LIMIT_MONTHLY="3"
TIMELINE_LIMIT_YEARLY="0"
```

Then, force cleanup based on the rules:

```bash
sudo snapper cleanup number
sudo snapper cleanup timeline
```

---

## Common pitfalls

### File deleted, but space not freed

Happens when a process still has the file open (file handle), even after it's been removed from the filesystem. The space is only freed when the process closes it or is restarted.

```bash
# Find deleted files still held open by processes
sudo find /proc/*/fd -ls 2>/dev/null | grep '(deleted)' | sort -rn -k7 | head -10
```

> [!tip] 
> `memfd` doesn't count Entries like `/memfd:something (deleted)` are shared memory segments (RAM), not disk files — they can be ignored in this context.

### `df` shows disk full, but `du` can't find anything

Possible causes, in order of likelihood:

1. Btrfs snapshots still being processed (cleaner)
2. Large file deleted but still open by a process
3. Subvolume exists but isn't mounted in the current tree (containers/VMs)
4. `du` run without sufficient permission (always use `sudo`)

---

## General system cleanup

```bash
# Package cache (apt)
sudo du -sh /var/cache/apt/archives
sudo apt clean

# Package cache (zypper)
sudo du -sh /var/cache/zypp
sudo zypper clean

# systemd journal logs
journalctl --disk-usage
sudo journalctl --vacuum-size=500M    # limits total size
sudo journalctl --vacuum-time=2weeks  # limits by age

# Old Snap package revisions
snap list --all | awk '/disabled/{print $1, $3}' | \
    while read snapname revision; do
        sudo snap remove "$snapname" --revision="$revision"
    done

# Limit snap to keep only the last 2 revisions
sudo snap set system refresh.retain=2

# Docker (unused images, containers, and volumes)
docker system df
docker system prune -a

# User cache and trash
du -sh ~/.cache ~/.local/share/Trash 2>/dev/null
rm -rf ~/.local/share/Trash/*
```

---

## Investigation checklist

- [ ] `df -h` — identify the full partition
- [ ] Confirm whether there are mount points/bind mounts on the same partition (e.g., `/home`, `/var`, `/opt` all on the same device)
- [ ] `sudo ncdu -x /` — browse visually (always with sudo)
- [ ] If Btrfs: `sudo btrfs filesystem usage /` to see real usage
- [ ] If Btrfs: check snapshots with `snapper list` and `btrfs filesystem du -s`
- [ ] Check `journalctl --disk-usage`
- [ ] Check package cache (`apt`/`zypper`)
- [ ] Check containers/VMs (Docker, libvirt, Distrobox)
- [ ] Check deleted files still open (`find /proc/*/fd`)
- [ ] After any major cleanup: `sync` or restart to ensure the space was actually returned

---

_Created from a real troubleshooting session on openSUSE Tumbleweed (Btrfs + Snapper)._