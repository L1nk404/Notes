```bash
1. ##########################
2. ## Working with files and directory (touch, mkdir, cp, mv, rm, shred)
3. ##########################

4. # creating a new file or updating the timestamps if the file already exists
5. touch filename

6. # creating a new directory
7. mkdir dir1

8. # creating a directory and its parents as well
9. mkdir -p mydir1/mydir2/mydir3

10. ######################
11. ### The cp command ###
12. ######################
13. # copying file1 to file2 in the current directory
14. cp file1 file2

15. # copying file1 to dir1 as another name (file2)
16. cp file1 dir1/file2

17. # copying a file prompting the user if it overwrites the destination
18. cp -i file1 file2

19. # preserving the file permissions, group and ownership when copying
20. cp -p file1 file2

21. # being verbose
22. cp -v file1 file2

23. # recursively copying dir1 to dir2 in the current directory
24. cp -r dir1 dir2/

25. # copy more source files and directories to a destination directory
26. cp -r file1 file2 dir1 dir2 destination_directory/

27. ######################
28. ### The mv command ###
29. ######################
30. # renaming file1 to file2
31. mv file1 file2

32. # moving file1 to dir1
33. mv file1 dir1/

34. # moving a file prompting the user if it overwrites the destination file
35. mv -i file1 dir1/

36. # preventing a existing file from being overwritten
37. mv -n file1 dir1/

38. # moving only if the source file is newer than the destination file or when the destination file is missing
39. mv -u file1 dir1/

40. # moving file1 to dir1 as file2
41. mv file1 dir1/file2

42. # moving more source files and directories to a destination directory
43. mv file1 file2 dir1/ dir2/ destination_directory/

44. ######################
45. ### The rm command ###
46. ######################
47. # removing a file
48. rm file1

49. # being verbose when removing a file
50. rm -v file1

51. # removing a directory
52. rm -r dir1/

53. # removing a directory without prompting
54. rm -rf dir1/

55. # removing a file and a directory prompting the user for confirmation
56. rm -ri fil1 dir1/

57. # secure removal of a file (verbose with 100 rounds of overwriting)
58. shred -vu -n 100 file1
```