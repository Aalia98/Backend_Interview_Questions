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

`rm file.txt` deletes a file. `rm -r folder` deletes a folder and everything inside (recursive). `rmdir folder` deletes only an empty folder. There is no recycle bin: deleted files are gone, so check the path first (`ls` before `rm`). `rm -i` asks before each delete.

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

`top` shows live running processes with CPU and memory use, sorted by CPU by default. Useful keys: `q` quit, `M` sort by memory, `P` sort by CPU, `k` kill a process. `htop` is a friendlier version (may need install).

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

`/` is the top-most directory of the Linux filesystem; everything lives under it. Do not confuse it with the **root user** (the superuser with full permissions) and with `/root` (the root user's home folder).

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

`grep` searches for text matching a pattern inside files or command output.
```bash
grep "error" log.txt
grep -i "error" log.txt      # ignore case
grep -n "error" log.txt      # line numbers
grep -r "TODO" ./src         # search folders
grep -v "debug" log.txt      # lines NOT matching
grep -c "error" log.txt      # count matches
```

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

`tar` bundles many files into one archive (and with `-z` compresses it).
```bash
tar -cvf files.tar folder/      # create
tar -czvf files.tar.gz folder/  # create + gzip
tar -xvf files.tar              # extract
tar -xzvf files.tar.gz          # extract gzip
```
Flags: c create, x extract, v verbose, f file name, z gzip.

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

### 51. `head`, `tail`, `wc`: quick file viewing

```bash
head -n 20 file.txt   # first 20 lines
tail -n 20 file.txt   # last 20 lines
wc -l file.txt        # count lines
```

### 52. `cp -r`, `mv`, `mkdir -p`

`cp -r dir1 dir2` copies a folder. `mv` moves or renames (same command). `mkdir -p a/b/c` creates nested folders in one go.

### 53. Important Linux directories: /bin, /usr, /tmp, /dev, /proc, /root

`/bin` essential commands, `/usr` installed programs, `/tmp` temporary files, `/dev` devices, `/proc` live system and process info, `/root` home of the root user, `/opt` optional software, `/mnt` mount points. (`/home`, `/etc`, `/var` are above.)

### 54. `which`, `whereis`, `man`, `--help`

`which go` shows where a command is. `man ls` opens the manual. `ls --help` gives a short help. Use these when you forget an option.

### 55. Editing files in the terminal: nano and vim basics

nano: easy, `Ctrl+O` save, `Ctrl+X` exit. vim: `i` insert mode, `Esc` normal mode, `:wq` save and quit, `:q!` quit without saving.

### 56. `echo`, `>` and `>>` for writing to files

`echo "hello" > a.txt` overwrites, `echo "more" >> a.txt` appends. `cat a.txt` to check.

### 57. Wildcards and shortcuts: `*`, `?`, `~`, `.`, Tab, Ctrl+C, Ctrl+R

`*` any characters (`*.log`), `?` one character, `~` your home folder, `.` current folder, `..` parent. Tab autocompletes, Ctrl+C stops a running command, Ctrl+R searches history, `!!` repeats the last command.

### 58. How do chmod numbers work? (755, 644, 600)

r=4, w=2, x=1; add them per group: owner, group, others. `7`=rwx, `6`=rw-, `5`=r-x, `4`=r--. So `755` = owner rwx, group and others r-x (scripts, folders). `644` = owner rw, others read (normal files). `600` = only owner (private keys). You can also write `chmod u+x file` (add execute for the user).

### 59. How do you read `ls -l` output?

`-rwxr-xr-- 1 aalia dev 120 Nov 16 app.sh`: first char is type (`-` file, `d` directory, `l` link); then three triplets for owner, group, others; then link count, owner, group, size, date, name.

### 60. Users and groups: `whoami`, `id`, `useradd`, `usermod`

`whoami` current user, `id` user and groups. Admin commands (with sudo): `useradd`, `passwd`, `usermod -aG group user` to add a user to a group. Files have an owner and a group; groups let teams share access.

### 61. Why do you get "Permission denied"? How do you debug it?

Check who you are (`whoami`), the file mode and owner (`ls -l`), and the permissions of parent folders. Fix with `chmod` or `chown` only as much as needed. For a script: it may need `chmod +x`. Do not use `777` as a quick fix.

### 62. How do you find and kill the process using a port?

```bash
ss -ltnp | grep :8080     # or: lsof -i :8080
kill PID                  # try normal kill first
```

### 63. `ps aux | grep myapp`: what is wrong with its output?

The `grep myapp` command itself shows up in the list. Use `pgrep -a myapp` or `ps aux | grep [m]yapp` to avoid it.

### 64. `kill` vs `killall` vs `pkill`

`kill PID` uses the process ID. `pkill name` / `killall name` use the process name (be careful, they can kill several processes).

### 65. Common signals: SIGTERM, SIGKILL, SIGINT, SIGHUP

SIGTERM (15) asks to stop gracefully (default of `kill`). SIGKILL (9) forces stop, cannot be caught. SIGINT (2) is Ctrl+C. SIGHUP (1) terminal closed (some services reload config). A Go server should handle SIGTERM for graceful shutdown.

### 66. `sort`, `uniq`, `cut`, `awk`, `sed`: the everyday text tools

```bash
sort file.txt | uniq -c | sort -nr      # count duplicates, most first
cut -d"," -f2 data.csv                  # 2nd column of a CSV
awk '{print $1}' access.log              # first field of every line
sed 's/old/new/g' file.txt               # replace text (prints result)
```

### 67. Example: find the top 5 IPs in an access log

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr | head -5
```
This is a favourite interview question: it combines pipes, awk, sort and uniq.

### 68. `less`, `cat`, `tail`: which to use for big files?

Never `cat` a huge log. Use `less` (scroll, `/word` to search, `q` to quit), `tail -n` for the end, `head` for the start, and `grep` to filter.

### 69. Where are logs stored?

Mostly under `/var/log` (system, nginx, auth). For systemd services use `journalctl -u service`. Application logs depend on the app (stdout, a file path in config).

### 70. How do you check disk, memory and CPU quickly?

`df -h` (disk), `du -sh *` (folder sizes), `free -h` (memory), `top` / `htop` (CPU and processes), `uptime` (load).

### 71. Cron: how do you schedule a job?

`crontab -e` and add a line: `minute hour day month weekday command`. Example: `30 2 * * * /home/app/backup.sh` runs every day at 2:30. Check with `crontab -l`. Use full paths and redirect output to a log file.

### 72. `ping`, `curl`, `wget`, `nslookup`/`dig`, `traceroute`: what does each do?

`ping host` checks reachability. `curl URL` makes an HTTP request (see answer above). `wget URL` downloads a file. `nslookup` / `dig domain` check DNS. `traceroute host` shows the network hops. `ip a` shows your IP addresses.

### 73. `ssh` and `scp`: how do you log in to and copy files to a server?

```bash
ssh user@server-ip
scp file.txt user@server-ip:/home/user/
scp user@server-ip:/var/log/app.log .
```
Use key-based login (`ssh-keygen`, `ssh-copy-id`) and keep the private key at `chmod 600`.

### 74. How do you deploy and keep a Go app running on Linux?

Build the binary (`go build`), copy it to the server, put config in environment variables, and run it as a **systemd service** (so it restarts on failure and starts on boot), or run it in Docker. Check `systemctl status` and `journalctl -u`. Put Nginx in front as a reverse proxy.

### 75. Basic shell script

```bash
#!/bin/bash
name="Aalia"
if [ -f /tmp/a.txt ]; then
  echo "exists"
else
  echo "hello $name"
fi
for f in *.log; do
  echo "$f"
done
```
Make it executable: `chmod +x script.sh`, run `./script.sh`. `$1` is the first argument, `$?` is the last exit status.