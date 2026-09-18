```bash
1. ##########################
2. ## Viewing files (cat, less, more, head, tail, watch)
3. ##########################

4. # displaying the contents of a file
5. cat filename

6. # displaying more files
7. cat filename1 filename2

8. # displaying the line numbers
9. cat -n filename

10. # concatenating 2 files
11. cat filename1 filename2 > filename3

12. # viewing a file using less
13. less filename

14. # less shortcuts:
15. # h => getting help
16. # q => quit
17. # enter => show next line
18. # space => show next screen
19. # /string => search forward for a string
20. # ?string => search backwards for a string
21. # n / N => next/previous appearance

22. # showing the last 10 lines of a file
23. tail filename

24. # showing the last 15 lines of a file
25. tail -n 15 filename

26. # showing the last lines of a file starting with line no. 5
27. tail -n +5 filename

28. # showing the last 10 lines of the file in real-time
29. tail -f filename

30. # showing the first 10 lines of a file
31. head filename

32. # showing the first 15 lines of a file
33. head -n 15 filename

34. # running repeatedly a command with refresh of 3 seconds
35. watch -n 3 ls -l
```