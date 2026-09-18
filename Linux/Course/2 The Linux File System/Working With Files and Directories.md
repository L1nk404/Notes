## Index
- [[#The `ls`command in Depth]]
- [[#Understanding File Timestamps `atime`, `mtime`, `ctime` (`stat`, `touch`, `date`)]]
	- [[#Changing File Timestamp]]
	- [[#Manipulating a File's Change Time `ctime`]]
	- [[#The `date` command]]
- [[#File Types in Linux (`ls -F`, `file`)]]
- [[#Viewing File (cat, tail, head, watch)]]
	- [[#`cat`]]
	- [[#`tail` and `head`]]
	- [[#`watch`]]
	- [[#Viewing Files Cheat Sheet]]
- [[#Manipulating Files andDirectories (`mkdir`, `cp`, `mv`, `rf`, `shred`)]]
	- [[#`mkdir` - make directory]]
	- [[#`cp` - copy]]
---
## The `ls`command in Depth
#ls


> [!NOTE] Cheat Sheet
> [[ls | `ls` cheat Sheet]]

![[Pasted image 20260716135147.png]]


> [!NOTE] Special type of files
> Beside `-`, `d` and `l` we have two special types of files:
> - `b`: Is a **block device**. A block device is a type of device file that allows for reading and writing data in fixed-size blocks, typically used for storage devices like hard drives and SSDs. Example: `brw-rw---- 1 root disk 8, 1 Jul 27 14:45 /dev/sda1`
> - `c`: Is a **char device**. in Linux is an abstraction that allows userspace applications to communicate with hardware or kernel subsystems by transmitting data sequentially **byte-by-byte as a continuous stream**. Unlike block devices (like hard drives) which read and write data in fixed-size chunks using caches, character devices provide unbuffered, sequential access. Example: `crw-rw-rw- 1 root root 1, 5 Jul 27 14:45 /dev/zero`
> - `s`:  Is a `socket`. This type of file is used by processes to communicate and has nothing to do with the socket term in TCP/IP. Example: `srw-rw-rw- 1 root root 0 Jul 27 14:45 /run/snapd-snap.socket`

### `ls` Cheat Sheet

---
## Understanding File Timestamps: `atime`, `mtime`, `ctime` (`stat`, `touch`, `date`)
#timestamp #atime #mtime #ctim

> [!NOTE] Cheat Sheet
> [[Timestamp]]

Every file on Linux has three timestamps:
1. The **Access** timestamp or `atime` is the last time the file was read (`ls -lu`)
2. The **Modified** timestamp or `mtime` is the last time the contents of the file was modified (`ls -l`, `ls -lt`)
3. The **Change** timestamp `ctime` is the last time when some metadata related to the file was changed (`ls -lc`)

> [!important] Change timestamp
> The Change timestamp refers to change on the **metadata** of the file, such the file permissions or file owner, it's not related to the file content

Linux file timestamps hold an integer number rather than a date and time. This number is the number of seconds since the unique epoch, which was **midnight on January, 1st, 1970 UTC - (00:00, 1/1/1970)**.
When Linux needs to display the date, it translates that number of seconds into a date and time.
To see these timestamps we can use the `ls`command or `stat` to see them all:

![[Pasted image 20260721153504.png]]

> We can use `grc` command to add color to it

To see the entire timestamp with the `ls` command, we use the `--full-time` parameter :

```bash
➜  ~ grc ls --full-time -lu /etc/passwd  
-rw-r--r-- 1 root root 2814 2026-07-21 01:19:19.785000061 +0000 /etc/passwd
```
### Changing File Timestamp
#changing_file_timestamp
This option is useful when you want a backup program to include or exclude some files or when you simply do not want other users to know that you've read or modified the file.
We can do that using the `touch` command.

> [!info] Note
>  If the file which is the argument of the `touch` command does not exist, it will create it. But if it exists, it will update the file timestamps to the computer's current time.

- **Changing the Access time** - `touch -a <file>`
- **Changing the Modification time** - `touch -m <file>`

To set a specific time we use `-t <year><month><day><hour><minute>.<second>` option. For example:

```bash
# Changing the modification time to 12/30/2018 - 15:30:45
touch -m -t 201812301530.45 linux.txt
```

And if we want to change simultaneously both **access** and **modifitification** time we use the `-d "<year>-<month>-<day> <hour>:<minute>:<second>" <file ` option:

```bash
touch -d "2010-10-31 15:45:30" linux.txt
```

We can also set a timestamp of a reference file to another:

```bash
➜  ~ stat ubuntu.txt    
 File: ubuntu.txt  
 size: 0               Blocks: 0          IO Block: 4096   regular empty file  
Device: 8,2     Inode: 1711295     Links: 1  
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    link)   Gid: ( 1000/    link)  
Access: 2026-07-21 19:00:24.065929149 +0000  
Modify: 2026-07-21 19:00:24.065929149 +0000  
Change: 2026-07-21 19:00:24.065929149 +0000  
Birth: 2026-07-21 19:00:24.065929149 +0000  
➜  ~ stat linux.txt    
 File: linux.txt  
 size: 0               Blocks: 0          IO Block: 4096   regular empty file  
Device: 8,2     Inode: 1710188     Links: 1  
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    link)   Gid: ( 1000/    link)  
Access: 2010-10-10 15:45:30.000000000 +0000  
Modify: 2010-10-10 15:45:30.000000000 +0000  
Change: 2026-07-21 18:59:08.201968522 +0000  
Birth: 2026-07-21 18:47:58.457756170 +0000  
➜  ~ touch linux.txt -r ubuntu.txt              

# ubuntu.txt is the reference file => linux.txt will copy the ubuntu.txt timestamp
➜  ~ stat linux.txt    
 File: linux.txt  
 size: 0               Blocks: 0          IO Block: 4096   regular empty file  
Device: 8,2     Inode: 1710188     Links: 1  
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    link)   Gid: ( 1000/    link)  
Access: 2026-07-21 19:00:24.065929149 +0000  
Modify: 2026-07-21 19:00:24.065929149 +0000  
Change: 2026-07-21 19:00:43.027919308 +0000  
Birth: 2026-07-21 18:47:58.457756170 +0000  
```

### Manipulating a File's Change Time `ctime`
#ctime

Note that `ctime` cannot be changed by the `touch` command. This happens because the kernel always stamps it with the **current** system time whenever anything on the inode changes.

Since `ctime` always reflects the system's current moment at the time of the change, you can "trick" it by temporarily changing the system clock:

1. **Set the system date/time** to the desired `ctime` value:

```bash
   sudo date -s "2026-07-20 15:30:00"
```

2. **Run `touch`** on the file (this updates `atime`, `mtime`, and `ctime`, all to the date you just set):

```bash
   touch file.txt
```

3. **Restore `atime` and `mtime`** to their original values, using `touch` with specific flags:

```bash
   touch -a -t <original_timestamp> file.txt   # restores access time
   touch -m -t <original_timestamp> file.txt   # restores modification time
```

4. **Set the system date/time back to the real value** (important — don't forget this):

```bash
   sudo date -s "$(date +'%Y-%m-%d %H:%M:%S')"
   # or resync via NTP:
   sudo timedatectl set-ntp true
```

> [!warning] Important
> A subtlety worth noting Since `ctime` only updates when something on the file changes, step 3 (restoring `atime`/`mtime`) will **also bump `ctime` again** — to whatever moment step 3 is actually run. So the order matters: **the final `ctime` will end up being the time at which step 3 was executed**, not the moment of step 2. Make sure the system clock is still set to your desired value when you run step 3, _before_ reverting it back in step 4.
### The `date` command
#date
Shows or sets the system's date and time.
#### Display current date/time

```bash
date              # local timezone
date -u           # UTC (Coordinated Universal Time)
```
#### Custom output format

```bash
date +"%Y-%m-%d %H:%M:%S"     # 2026-07-21 14:32:10
date +"%d/%m/%Y"              # 21/07/2026
date +%s                      # Unix timestamp (seconds since epoch)
```

Common format codes:

|Code|Meaning|
|---|---|
|`%Y`|Year (4 digits)|
|`%y`|Year (2 digits)|
|`%m`|Month (01-12)|
|`%d`|Day (01-31)|
|`%H`|Hour, 24h (00-23)|
|`%M`|Minute|
|`%S`|Second|
|`%s`|Unix timestamp|
|`%A`|Full weekday name|
|`%B`|Full month name|
- **Set the system date/time**

```bash
sudo date -s "2026-07-21 15:30:00"
```

> [!warning] 
> Requires root Setting the system clock needs `sudo`. Also, if a time sync service (like `systemd-timesyncd` or `chrony`) is running, it may override your manual change shortly after.

- **Convert a Unix timestamp to a readable date**

```bash
date -d @1783962827
```

- **Restore time sync after a manual change**

```bash
sudo timedatectl set-ntp true
```

- **Check the manual for accepted date formats**

```bash
man date
```

---
## File Types in Linux (`ls -F`, `file`)
#file
Linux determines the type of a file via a code in the file header. It  does not depend in the file extension.
We can use `file` to determine the header of the file that contains their type.

> Note that it can be changed

```bash
➜  ~ file linux.txt  
linux.txt: ASCII text
```

We can also use `ls -F` command:
- No symbol added - Indicates a regular non-executable file: `linux.txt`
- `/` - Indicated a directory: `Documents/`
- `@` - Indicates a symlink:  `os-release@`
- `=` - Indicates a socket: `snapd.socket=`
- `*` - Indicates an executable file: `zstd*`
---
## Viewing File (cat, tail, head, watch)


> [!NOTE] Cheat Sheet
> [[Cat Tail Head Watch|cat, tail, head, watch cheat sheet]]
### `cat`
#cat
Cat allows to open files. We can use the `-n` parameter to see the number of the lines of a file:

```bash
➜  ~ cat -n /etc/group  
    1  root:x:0:  
    2  daemon:x:1:  
    3  bin:x:2:  
    4  sys:x:3:  
    5  adm:x:4:syslog,link  
    6  tty:x:5:  
    7  disk:x:6:  
    8  lp:x:7:  
    9  mail:x:8:
```
### `tail` and `head`
#tail #head
`tail` allows to see the last lines of a file while `head` reads the top lines.
By default they read the last/first 10 lines. 
#### Usefull Parameters
- `-n <number>` - show the last $n$ lines;
	- We can use `+` to see the last lines starting from a specific line:
```bash
➜  ~ tail -n  +8 /etc/group    
lp:x:7:  
mail:x:8:  
news:x:9:  
uucp:x:10:  
man:x:12:  
proxy:x:13:  
kmem:x:15:  
dialout:x:20:  
fax:x:21:  
voice:x:22:  
cdrom:x:24:link  
floppy:x:25:
```
- Tail is really usfeul for reading a file in real time, like when you want see the last lines of a log file:
	- For that we use `-f` parameter, for example `tail -f /var/log/auth.log` :

![[Pasted image 20260805225116.png]] 
### `watch`  
#watch
`watch` runs command repeatedly, displaying its output and errors (the first screenfull). This  allows  you to watch the program output change over time. By default, command is run every 2 seconds and watch will run until interrupted.
#### Usefull Parameters
- `-d` - highlight the differences between succesisve updates 
- `-n <number>` - specify the update interval

--- 
## Manipulating Files and Directories (`mkdir`, `cp`, `mv`, `rf`, `shred`)


> [!NOTE] Cheat Sheet
> [[touch, mkdir, cp, mv, rm, shred] | touch, mkdir, cp, mv, rm, shred Cheat Sheet]]

### `mkdir` - make directory
#mkdir

`mkdir` is a tool to create directories
#### Useful parameteres:
- We can use `-p` to create a whole structure of directories in one command:

```bash
➜  ~ mkdir -p -v  /tmp/first/second/third # -v stands for verbose  
mkdir: created directory '/tmp/first'  
mkdir: created directory '/tmp/first/second'  
mkdir: created directory '/tmp/first/second/third'
```

`-p` options is also good when the directory exists and you don't want an error:

```bash
➜  ~ mkdir /tmp/dir1  
mkdir: /tmp/dir1: File exists  
➜  ~ mkdir -p /tmp/dir1
```
### `cp` - copy
#cp

`cp` have three principal modes of operation, depending on the number and the type of arguments passed.
1. If the command has two arguments of type files, it copies the content of the first file to the second file. **If the second file does not exist it will be create, if exists, it will overwrite the destination**:

```bash
➜  ~ cp -v /etc/group ./users.txt # -v stands for verbose   
'/etc/group' -> './users.txt'
```
- It's recommend to use `-i` to prompt confirmation if the file exists:

```bash 
➜  ~ cp -v -i /etc/passwd ./users.txt     
cp: overwrite './users.txt'? y  
'/etc/passwd' -> './users.txt'
```

2. If the command has one or more arguments of type files, which the source and the last argument is a directory, which is a destination. It copies the source files to the directory.
```bash
➜  ~ cp -v -i /etc/group /etc/passwd /etc/dracut.conf /tmp  
'/etc/group' -> '/tmp/group'  
'/etc/passwd' -> '/tmp/passwd'  
'/etc/dracut.conf' -> '/tmp/dracut.conf'
```

3. If all the arguments are of type directory, cp copies all files in the source directory to the directory directory creating any files or direcotry needed. This requires `-r` parameter:
```bash
➜  ~ sudo cp -r /etc/ /tmp  # copies the entire /etc/ directory into /tmp
➜  ~ ls -l /tmp          
total 12  
drwxrwxr-x   2 link link   40 Aug  6 02:17 dir1  
-rw-r--r--   1 link link  117 Aug  6 02:28 dracut.conf  
drwxr-xr-x 144 root root 4960 Aug  6 02:33 etc  
drwxrwxr-x   3 link link   60 Aug  6 02:15 first  
-rw-r--r--   1 link link 1169 Aug  6 02:28 group  
drwxr-x---  18 link link  820 Aug  6 02:31 link  
-rw-r--r--   1 link link 2814 Aug  6 02:28 passwd
```


> [!warning] Warning
> When a user copies a file from another user, it becomes the owner.
> To preserve the file attributes, permissions, group and user ownership use the `-p`option.


> [!NOTE] 
> Is possible to copy one or more files from only one source, for that, we use `tee`:
```bash
tee dest_file1 dest_file2 < file_source
```
### `mv`





