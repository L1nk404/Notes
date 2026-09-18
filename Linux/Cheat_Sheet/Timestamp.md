```bash
1. ##########################
2. ## File Timestamps and Date
3. ##########################

4. # displaying atime
5. ls -lu

6. # displaying mtime
7. ls -l
8. ls -lt

9. # displaying ctime
10. ls -lc

11. # displaying all timestamps
12. stat file.txt

13. # displaying the full timestamp
14. ls -l --full-time /etc/

15. # creating an empty file if it does not exist, update the timestamps if the file exists
16. touch file.txt

17. # changing only the access time to current time
18. touch -a file

19. # changing only the modification time to current time
20. touch -m file

21. # changing the modification time to a specific date and time
22. touch -m -t 201812301530.45 a.txt

23. # changing both atime and mtime to a specific date and time
24. touch -d "2010-10-31 15:45:30" a.txt

25. # changing the timestamp of a.txt to those of b.txt
26. touch a.txt -r b.txt

27. # displaying the date and time
28. date

29. # showing this month's calendar
30. cal

31. # showing the calendar of a specific year
32. cal 2021

33. # showing the calendar of a specific month and year
34. cal 7 2021

35. # showing the calendar of previous, current and next month
36. cal -3

37. # setting the date and time
38. date --set="2 OCT 2020 18:00:00"

39. # displaying the modification time and sorting the output by name.
40. ls -l

41. # displaying the output sorted by modification time, newest files first
42. ls -lt

43. # displaying and sorting by atime
44. ls -ltu

45. # reversing the sorting order
46. ls -ltu --reverse
```