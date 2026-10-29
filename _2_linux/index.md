# 1. What does pwd do?

- Prints the current working directory (your present location in filesystem).
- pwd

# 2. Difference between ls and ls -la?

- ls → lists visible files
- ls -la → lists all files including hidden + permissions, owner, size, date

# 3. What does cd .. do?

- Moves one directory up (to parent directory).

# 4. Create a directory

- mkdir myFolder

# 5. Create a file

- touch file.txt

# 6. Delete file and folder

- rm file.txt
- rm -r folderName

# 7. Copy a file

- cp source.txt dest.txt

# 8. Move / rename a file

- mv old.txt new.txt

# 9. View file content

- cat file.txt
- less file.txt

# 10. What does clear do?

- Clears the terminal screen.

# 11. What does chmod do?

- Changes file permissions (read, write, execute).
- chmod 755 file.sh

# 12. Difference between r, w, x?

- r = read
- w = write
- x = execute

# 13. What is sudo?

- Runs a command as superuser (admin).

# 14. What is a process?

- A running instance of a program.

# 15. Kill a running process

- kill PID
- kill -9 PID

# 16. What is ps?

- Shows running processes.

# 17. What is top?

- Displays real-time running processes and system usage.

# 18. Foreground vs background?

- Foreground: occupies terminal
- Background: runs behind using &

# 19. What does & do?

- Runs a process in background.
- node server.js &

# 20. What is kill -9?

- Forcefully terminates a process.

# 21. What is root /?

- Top-most directory in Linux.

# 22. Absolute vs relative path?

- Absolute: starts from / → /home/user/file.txt
- Relative: from current dir → ../file.txt

# 23. What is /home?

- Stores user home directories.

# 24. What is /etc?

- System configuration files.

# 25. What is /var?

- Variable files like logs, cache.

# 26. What is grep?

- Search text in files.
- grep "error" log.txt

# 27. What is pipe |?

- Pass output of one command to another.
- ps aux | grep node

# 28. What is history?

- Shows previously run commands.

# 29. Difference between rm and rm -rf?

- rm: remove file
- rm -rf: force delete folder recursively

# 30. What is tar?

- Used to archive files.
- tar -cvf files.tar folder/