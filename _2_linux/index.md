## Linux

### 1. What does pwd do?

- Prints the current working directory (your present location in filesystem).
- pwd

### 2. Difference between ls and ls -la?

- ls → lists visible files
- ls -la → lists all files including hidden + permissions, owner, size, date

### 3. What does cd .. do?

- Moves one directory up (to parent directory).

### 4. Create a directory

- mkdir myFolder

### 5. Create a file

`touch file.txt` updates a file's access and modification times. If the file is missing, it normally creates an empty file. It does not clear existing content. Use `touch -c` to avoid creating a missing file.

### 6. Delete file and folder

- rm file.txt
- rm -r folderName

### 7. Copy a file

- cp source.txt dest.txt

### 8. Move / rename a file

- mv old.txt new.txt

### 9. View file content

- cat file.txt
- less file.txt

### 10. What does clear do?

- Clears the terminal screen.

### 11. What does chmod do?

chmod changes read, write and execute permissions. `chmod 755 file.sh` gives rwx to the owner and r-x to group and others. Before changing permissions, check the owner and parent folders too. Do not use 777 as a default fix.

### 12. Difference between r, w, x?

For a file, r means read, w means write and x means execute.

For a directory, r lists names, w allows adding/removing entries along with the other required permissions, and x allows entering or searching it. Parent-directory permissions matter too.

### 13. What is sudo?

sudo lets an allowed user run a command as another user. Root is the usual default. `sudo -u appuser COMMAND` selects appuser if the policy allows it. sudo is not a fix for every permission error.

### 14. What is a process?

- A running instance of a program.

### 15. Kill a running process

Check the PID first. Try `kill PID`, which normally sends SIGTERM. The program can handle it and clean up. Use `kill -9 PID` only as a last step because SIGKILL does not allow cleanup.

```bash
kill PID
# Last resort; cleanup handlers cannot run:
kill -9 PID
```

### 16. What is ps?

ps shows a snapshot of selected processes. Plain ps normally shows processes for the current effective user and terminal. Use `ps -ef` or `ps aux` for a wider list.

```bash
ps -ef
ps -p PID -o pid,ppid,stat,%cpu,%mem,cmd
```

### 17. What is top?

- Displays real-time running processes and system usage.

### 18. Foreground vs background?

A foreground job uses the terminal for input and job control. A background job lets the shell accept more commands, but it can still print to the terminal. It is not automatically a service or safe from logout.

### 19. What does & do?

`&` starts a job in the background. It does not keep it alive after every logout or restart it after failure.

```bash
node server.js &
```

### 20. What is kill -9?

`kill -9 PID` sends SIGKILL. The process cannot catch or ignore it, so cleanup code cannot run. Try SIGTERM first. A task stuck in uninterruptible kernel sleep may not disappear immediately even after SIGKILL.

### 21. What is root /?

- Top-most directory in Linux.

### 22. Absolute vs relative path?

- Absolute: starts from / → /home/user/file.txt
- Relative: from current dir → ../file.txt

### 23. What is /home?

- Stores user home directories.

### 24. What is /etc?

- System configuration files.

### 25. What is /var?

- Variable files like logs, cache.

### 26. What is grep?

- Search text in files.
- grep "error" log.txt

### 27. What is pipe |?

A pipe sends stdout from one command to stdin of the next. stderr is separate unless redirected.

```bash
ps aux | grep node
command1 2>&1 | command2
```

### 28. What is history?

- Shows previously run commands.

### 29. Difference between rm and rm -rf?

rm removes files. `-r` removes directories recursively. `-f` avoids prompts and ignores missing files; it does not give extra permissions. `rm -rf` can delete a lot of data. Check the exact target before using it.

### 30. What is tar?

- Used to archive files.
- tar -cvf files.tar folder/

### 31. What are stdin, stdout and stderr? How do you redirect them?

stdin, stdout and stderr are the standard input, output and error streams. Their file descriptors are 0, 1 and 2. `>` replaces output, `>>` appends and `2>` redirects errors.

Order matters: `>out.log 2>&1` puts both streams in the file. `2>&1 >out.log` leaves stderr going to the old stdout destination.

```bash
command >out.log 2>err.log
command >>combined.log 2>&1
```

### 32. What does $? mean? How can a failed pipeline look successful?

`$?` holds the last command or pipeline exit status. Zero normally means success; check the command's own meaning for other values. Save it before running another command.

A pipeline normally returns the last command's status. In Bash, `set -o pipefail` returns the rightmost non-zero status, or zero if all commands succeed.

```bash
set -o pipefail
command1 | command2
status=$?
```

### 33. find vs grep: how do you search for files and their content?

find searches filesystem entries by name, type, age or size. grep searches matching lines inside files. Quote find patterns so the shell does not expand them first. `grep -i` ignores case and `-n` shows line numbers.

```bash
find ./logs -type f -name "*.log"
grep -n -i "timeout" app.log
grep -r -n "TODO" ./src
```

### 34. How do you follow logs? tail -f vs tail -F?

`tail -n 100` shows the last 100 lines. `tail -f` normally follows the open file. GNU `tail -F` follows the filename and retries, so it helps when logs rotate. For a systemd service, journalctl can show logs by service and time.

```bash
tail -n 100 app.log
tail -F app.log
journalctl -u myapi.service --since "30 min ago"
journalctl -u myapi.service -f
```

### 35. How do you manage a service? start vs enable vs restart?

On systemd, start runs a service now. enable sets it up for later activation, often at boot; it does not necessarily start it now. `enable --now` does both. restart stops and starts it, so it can interrupt traffic. Check status and logs before changing a running service.

```bash
systemctl status myapi.service
systemctl is-active myapi.service
# A restart changes a running service; do not practise
# this on a shared or production host.
```

### 36. chmod vs chown? What does umask do?

chmod changes permission bits. chown changes owner/group. umask masks permissions for newly created files and directories; it does not change old ones.

For example, requested mode 0666 with mask 022 gives 0644. For directories, 0777 with 022 gives 0755 when no default ACL changes the result. This is bit masking, not normal decimal subtraction.

```bash
ls -l FILE
# Example mode, not a universal permission fix:
chmod 640 FILE
umask
```

### 37. Hard link vs symbolic link?

A hard link is another name for the same file inode, normally on the same filesystem. A symbolic link stores a target path, can cross filesystems and can point to a missing target. Removing one hard-link name does not remove the data while another link or open file still holds it.

```bash
ln original.txt hardlink.txt
ln -s original.txt symlink.txt
```

### 38. What is an inode? Can a disk be full while bytes remain?

An inode stores file details like owner, permissions and timestamps. The filename is stored in a directory entry. Many small files can use all available inodes even when space in bytes remains. Check `df -i` for inodes and `df -h` for space.

```bash
df -h
df -i
```

### 39. df vs du: what do they measure?

df shows filesystem space use. du measures storage used by selected files/directories. Their values can differ because of deleted open files, filesystem overhead, mounts, sparse files or permission errors. A mismatch alone does not prove a leak.

```bash
df -h /var
du -sh /var/log
du -h --max-depth=1 ./logs
```

### 40. You deleted a huge log but free space did not return. Why?

A process may still have the deleted file open. The space is released after the last open reference closes. Find the process and arrange safe log reopening, rotation or a controlled restart. Do not kill the service without checking.

### 41. How do you check memory? Does low free memory prove a leak?

Check `free -h` and top. Linux uses memory for cache, so low free memory alone does not mean a leak. Look at available memory, process memory trends, swap and OOM messages.

```bash
free -h
top
```

### 42. What is load average? Is it CPU percentage?

Load average is not CPU percentage. On Linux, it includes runnable/running tasks and tasks in uninterruptible sleep, over 1, 5 and 15 minutes. Compare it with CPU count and CPU/I/O activity. A high value can come from CPU work or I/O waits.

```bash
uptime
top
```

### 43. How do you check which process is listening on a port?

Use ss to inspect sockets. `-l` means listening, `-t` TCP, `-u` UDP, `-n` numeric addresses and `-p` process details when allowed. Check the bound address too: a loopback-only listener is not directly reachable from another machine.

```bash
ss -ltnp 'sport = :8080'
```

### 44. How do you test an API from the terminal?

Use curl to inspect the status, headers and response. `-i` includes headers, `--connect-timeout` limits connection setup and `--max-time` limits the total request. An HTTP error response is different from DNS, connection or TLS failure.

```bash
curl -i --connect-timeout 3 --max-time 10 \
  http://127.0.0.1:8080/health
```

### 45. A service works locally but not from another machine. What do you check?

Check the process, listening port and bound address first. Then check DNS, firewall rules and any proxy/load balancer. Compare a local request with a remote one and read the logs. A successful ping does not prove the API port works.

### 46. Shell variable vs environment variable? What does export do?

A shell variable belongs to that shell. export passes it to future child commands. It does not update a parent shell or an already running program. PATH is used to find executables. A service can have a different environment from the terminal. Do not print secret values while debugging.

```bash
APP_ENV=dev
export APP_ENV
APP_ENV=test ./myapi
```

### 47. jobs, fg, bg and Ctrl+Z: what do they do?

In an interactive Bash shell, jobs lists that shell's jobs. Ctrl+Z normally suspends the foreground job; it does not end it. bg resumes a stopped job in the background and fg brings it to the foreground. `%1` means shell job 1, not PID 1.

```bash
jobs
bg %1
fg %1
```

### 48. Will ./myapi & keep running after logout?

Not always. `&` only starts the command in the background. nohup ignores SIGHUP and handles terminal redirection, but does not itself start the command in the background or restart it after failure. Use a service manager for a deployed API.

```bash
nohup ./myapi >app.log 2>&1 &
```

### 49. What is a zombie process? Is it still consuming CPU?

A zombie is a child process that has already ended, but its parent has not collected the exit status with wait/waitpid. It is not running code or using CPU. Killing the zombie cannot end it again; check why the parent is not reaping children.

### 50. A Go API is slow on a Linux server. What do you do first?

First check which requests became slow and when it started. Read service logs, then inspect CPU/load, memory/swap, disk space/inodes and sockets. Test the API with a timeout and compare local and remote requests. Check DB and external-call timings before deciding the cause.

```bash
systemctl status myapi.service
journalctl -u myapi.service --since "30 min ago"
uptime
free -h
df -h
df -i
```