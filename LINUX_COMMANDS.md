# 🐧 Comprehensive Linux Commands Reference

> Every major Linux command with real-world examples.
> Tested on Debian/Ubuntu, RHEL/CentOS/Fedora, and Arch-based distros.

---

## Table of Contents

1. [Navigation & Directory Management](#1-navigation--directory-management)
2. [File Operations](#2-file-operations)
3. [Viewing & Reading Files](#3-viewing--reading-files)
4. [Searching & Finding](#4-searching--finding)
5. [Text Processing](#5-text-processing)
6. [User & Permission Management](#6-user--permission-management)
7. [Process Management](#7-process-management)
8. [Disk & Storage](#8-disk--storage)
9. [Networking](#9-networking)
10. [Package Management](#10-package-management)
11. [System Information](#11-system-information)
12. [Services & Systemd](#12-services--systemd)
13. [Archiving & Compression](#13-archiving--compression)
14. [SSH & Remote Access](#14-ssh--remote-access)
15. [Scheduling (cron & at)](#15-scheduling-cron--at)
16. [Environment & Shell](#16-environment--shell)
17. [Hardware & Devices](#17-hardware--devices)
18. [Kernel & Modules](#18-kernel--modules)
19. [Logging](#19-logging)
20. [Security & Firewall](#20-security--firewall)
21. [Performance Monitoring](#21-performance-monitoring)
22. [Miscellaneous Utilities](#22-miscellaneous-utilities)

---

## 1. Navigation & Directory Management

### `pwd` — Print Working Directory
```bash
pwd
# Output: /home/alice/projects
```

### `cd` — Change Directory
```bash
cd /var/log          # absolute path
cd Documents         # relative path
cd ~                 # home directory
cd -                 # previous directory
cd ../..             # two levels up
```

### `ls` — List Directory Contents
```bash
ls                   # basic listing
ls -l                # long format (permissions, size, date)
ls -la               # include hidden files
ls -lh               # human-readable sizes
ls -lt               # sort by modification time (newest first)
ls -lS               # sort by file size (largest first)
ls -R                # recursive listing
ls /var/log/*.log    # glob filter
```

### `mkdir` — Make Directory
```bash
mkdir new_folder
mkdir -p projects/webapp/src   # create parents as needed
mkdir -m 755 public            # create with specific permissions
```

### `rmdir` — Remove Empty Directory
```bash
rmdir empty_folder
rmdir -p a/b/c       # remove chain of empty directories
```

### `tree` — Directory Tree
```bash
tree                 # print full tree
tree -L 2            # limit depth to 2
tree -a              # include hidden files
tree -d              # directories only
tree -h              # human-readable sizes
```

---

## 2. File Operations

### `touch` — Create File or Update Timestamp
```bash
touch newfile.txt
touch -t 202301011200 file.txt   # set specific timestamp
touch file1 file2 file3          # multiple files
```

### `cp` — Copy
```bash
cp file.txt backup.txt
cp -r source_dir/ dest_dir/      # recursive (directories)
cp -p file.txt backup.txt        # preserve permissions & timestamps
cp -u file.txt dest/             # copy only if source is newer
cp -v file.txt dest/             # verbose output
cp -a source/ dest/              # archive mode (preserves all metadata)
```

### `mv` — Move or Rename
```bash
mv old.txt new.txt               # rename file
mv file.txt /tmp/                # move to directory
mv -i file.txt dest/             # interactive (prompt before overwrite)
mv -n file.txt dest/             # do not overwrite existing
mv *.log /var/archive/           # move all log files
```

### `rm` — Remove
```bash
rm file.txt
rm -i file.txt           # prompt before deleting
rm -f file.txt           # force (no prompt)
rm -r directory/         # recursive
rm -rf /tmp/scratch/     # recursive force (use carefully)
rm -- -oddfilename.txt   # remove files starting with -
```

### `ln` — Links
```bash
ln -s /etc/nginx/nginx.conf nginx.conf   # symbolic (soft) link
ln file.txt hardlink.txt                 # hard link
ls -la                                   # shows -> target for symlinks
readlink nginx.conf                      # show symlink target
```

### `stat` — File Status
```bash
stat file.txt
# Shows: size, blocks, inode, permissions, access/modify/change times
```

### `file` — Determine File Type
```bash
file archive.tar.gz       # archive.tar.gz: gzip compressed data
file script.sh            # script.sh: Bourne-Again shell script
file image.png            # image.png: PNG image data
```

### `truncate` — Shrink or Extend File Size
```bash
truncate -s 0 app.log    # empty a log file without deleting it
truncate -s 100M bigfile  # create a 100 MB sparse file
```

### `shred` — Securely Delete a File
```bash
shred -u -z -v secret.txt
# -u = remove after shredding, -z = overwrite with zeros, -v = verbose
```

---

## 3. Viewing & Reading Files

### `cat` — Concatenate and Display
```bash
cat file.txt
cat -n file.txt           # show line numbers
cat -A file.txt           # show non-printing characters (tabs, line endings)
cat file1.txt file2.txt   # concatenate two files
cat > newfile.txt         # create file from stdin (Ctrl+D to save)
cat >> existing.txt       # append to file from stdin
```

### `less` — Page Through a File
```bash
less /var/log/syslog
# Navigation: Space=next page, b=prev page, /=search, n=next match, q=quit
less +F /var/log/syslog   # follow mode (like tail -f, Ctrl+C to stop)
less -N /var/log/syslog   # show line numbers
```

### `more` — Simple Pager
```bash
more file.txt
# Space=next page, q=quit (simpler than less)
```

### `head` — First Lines of a File
```bash
head file.txt              # first 10 lines (default)
head -n 25 file.txt        # first 25 lines
head -c 100 file.txt       # first 100 bytes
```

### `tail` — Last Lines of a File
```bash
tail file.txt              # last 10 lines (default)
tail -n 50 file.txt        # last 50 lines
tail -f /var/log/syslog    # follow live updates
tail -f -n 100 app.log     # last 100 lines then follow
```

### `wc` — Word Count
```bash
wc file.txt                # lines, words, bytes
wc -l file.txt             # line count only
wc -w file.txt             # word count only
wc -c file.txt             # byte count only
wc -m file.txt             # character count (handles multibyte)
ls *.py | wc -l            # count number of Python files
```

### `diff` — Compare Files
```bash
diff file1.txt file2.txt
diff -u file1.txt file2.txt    # unified diff (standard patch format)
diff -r dir1/ dir2/            # compare directories recursively
diff --color file1 file2       # colorized output
```

### `od` — Octal Dump
```bash
od -c file.bin             # print characters
od -x file.bin             # print hex
od -An -tx1 file.bin       # hex without address
```

### `xxd` — Hex Dump
```bash
xxd file.bin
xxd -l 64 file.bin         # first 64 bytes
xxd -r hex.txt binary.bin  # reverse (hex back to binary)
```

---

## 4. Searching & Finding

### `find` — Search for Files
```bash
find /home -name "*.txt"                    # by name (case-sensitive)
find /home -iname "*.txt"                   # case-insensitive
find . -type f                              # files only
find . -type d                              # directories only
find . -type l                              # symlinks only
find . -mtime -7                            # modified in last 7 days
find . -mtime +30                           # modified more than 30 days ago
find . -size +100M                          # larger than 100 MB
find . -size -10k                           # smaller than 10 KB
find . -empty                               # empty files or directories
find /var/log -name "*.log" -delete         # find and delete
find . -name "*.py" -exec grep -l "def" {} \;  # find then exec
find . -perm 644                            # files with exact permissions
find . -user alice                          # owned by user alice
find . -newer reference.txt                 # newer than reference file
find / -maxdepth 3 -name "config"           # limit search depth
```

### `locate` — Fast File Search (Uses Database)
```bash
locate filename.txt
locate -i filename.txt       # case-insensitive
locate -c filename           # count matches only
sudo updatedb                # update the locate database
```

### `which` — Find Command Executable
```bash
which python3      # /usr/bin/python3
which -a python3   # all matching executables in PATH
```

### `whereis` — Find Binary, Source, and Man Page
```bash
whereis nginx
# nginx: /usr/sbin/nginx /etc/nginx /usr/share/man/man8/nginx.8.gz
```

### `type` — Describe a Command
```bash
type ls         # ls is aliased to `ls --color=auto`
type cd         # cd is a shell builtin
type python3    # python3 is /usr/bin/python3
```

---

## 5. Text Processing

### `grep` — Search Text
```bash
grep "error" app.log
grep -i "error" app.log          # case-insensitive
grep -n "error" app.log          # show line numbers
grep -r "TODO" ./src             # recursive search
grep -v "debug" app.log          # invert match (exclude)
grep -c "error" app.log          # count matching lines
grep -l "TODO" *.py              # list filenames with matches
grep -w "fail" app.log           # whole-word match
grep -A 3 "error" app.log        # 3 lines after match
grep -B 3 "error" app.log        # 3 lines before match
grep -C 3 "error" app.log        # 3 lines before and after
grep -E "error|warn" app.log     # extended regex (ERE)
grep -P "^\d{4}" file.txt        # Perl-compatible regex
grep --color=auto "error" app.log
```

### `sed` — Stream Editor
```bash
sed 's/foo/bar/' file.txt           # replace first occurrence per line
sed 's/foo/bar/g' file.txt          # replace all occurrences
sed -i 's/foo/bar/g' file.txt       # in-place edit
sed -i.bak 's/foo/bar/g' file.txt   # in-place with backup
sed -n '5,10p' file.txt             # print lines 5-10
sed '3d' file.txt                   # delete line 3
sed '/^$/d' file.txt                # delete empty lines
sed '/pattern/d' file.txt           # delete lines matching pattern
sed -n '/start/,/end/p' file.txt    # print between markers
sed 's/^/    /' file.txt            # indent each line by 4 spaces
sed '1i\new first line' file.txt    # insert line before line 1
sed '$a\new last line' file.txt     # append line after last line
```

### `awk` — Text Processing and Reporting
```bash
awk '{print $1}' file.txt                    # print first column
awk '{print $NF}' file.txt                   # print last column
awk -F: '{print $1}' /etc/passwd             # custom delimiter (:)
awk 'NR==5' file.txt                         # print line 5
awk 'NR>=5 && NR<=10' file.txt               # print lines 5-10
awk '{sum+=$1} END{print sum}' numbers.txt   # sum a column
awk '{print NR": "$0}' file.txt              # add line numbers
awk '/pattern/' file.txt                      # filter by pattern
awk '$3 > 100 {print $1, $3}' data.txt       # conditional print
awk 'BEGIN{print "Start"} {print} END{print "Done"}' file.txt
awk -F, '{OFS="\t"; $1=$1; print}' csv.txt  # convert CSV to TSV
```

### `cut` — Extract Columns
```bash
cut -d: -f1 /etc/passwd         # field 1, colon-delimited
cut -d, -f2,4 data.csv          # fields 2 and 4, comma-delimited
cut -c1-10 file.txt             # characters 1-10 of each line
cut -c5- file.txt               # from character 5 to end of line
```

### `sort` — Sort Lines
```bash
sort file.txt                   # alphabetical
sort -r file.txt                # reverse
sort -n numbers.txt             # numeric sort
sort -rn numbers.txt            # reverse numeric
sort -u file.txt                # unique (remove duplicates)
sort -k2 data.txt               # sort by 2nd field
sort -t: -k3 -n /etc/passwd     # sort /etc/passwd by UID (field 3)
sort -h sizes.txt               # human-readable numeric sort (1K, 2M, etc.)
```

### `uniq` — Report or Omit Repeated Lines
```bash
sort file.txt | uniq            # remove consecutive duplicates
sort file.txt | uniq -c         # count occurrences
sort file.txt | uniq -d         # show only duplicates
sort file.txt | uniq -u         # show only unique lines
```

### `tr` — Translate or Delete Characters
```bash
echo "hello" | tr 'a-z' 'A-Z'       # to uppercase
echo "hello world" | tr ' ' '_'      # replace spaces with underscores
cat file.txt | tr -d '\r'            # remove carriage returns (Windows→Unix)
cat file.txt | tr -s ' '            # squeeze multiple spaces into one
echo "abc123" | tr -d '0-9'         # delete digits
```

### `paste` — Merge Lines Side by Side
```bash
paste file1.txt file2.txt            # side by side with tab
paste -d, file1.txt file2.txt        # comma-delimited
paste -s file.txt                    # merge all lines into one
```

### `join` — Join Files on a Common Field
```bash
join file1.txt file2.txt             # join on first field (must be sorted)
join -t: -1 1 -2 1 file1 file2      # join on specified fields
```

### `tee` — Read stdin, Write to stdout and File
```bash
ls -la | tee listing.txt             # display and save
ls -la | tee -a listing.txt          # display and append
```

### `xargs` — Build and Execute Commands
```bash
find . -name "*.tmp" | xargs rm
find . -name "*.txt" | xargs grep "error"
echo "file1 file2 file3" | xargs -n1 touch   # one argument per command
ls *.jpg | xargs -I{} cp {} /backup/{}        # use placeholder
find . -name "*.py" | xargs -P4 pylint        # parallel execution (4 procs)
```

### `column` — Format Output in Columns
```bash
column -t data.txt                    # align columns
mount | column -t                     # neatly aligned mount output
cat /etc/passwd | column -t -s:      # colon-delimited, tabular
```

### `jq` — JSON Processor
```bash
cat data.json | jq '.'               # pretty-print JSON
cat data.json | jq '.name'           # extract field
cat data.json | jq '.users[0].email' # nested field
cat data.json | jq '.[] | .id'       # iterate array
curl -s https://api.github.com/users/linux | jq '{login,name,public_repos}'
cat data.json | jq 'length'          # count elements
cat data.json | jq 'keys'            # list keys of an object
```

---

## 6. User & Permission Management

### `whoami` / `id` — Current User
```bash
whoami              # current username
id                  # uid, gid, groups
id alice            # info for another user
```

### `useradd` / `adduser` — Create Users
```bash
useradd -m alice                    # create user with home directory
useradd -m -s /bin/bash -G sudo alice  # with shell and group
useradd -u 1500 -m alice            # specific UID
adduser alice                       # interactive (Debian/Ubuntu)
```

### `usermod` — Modify User
```bash
usermod -aG docker alice            # add to docker group
usermod -aG sudo alice              # grant sudo
usermod -s /bin/zsh alice           # change shell
usermod -l newname oldname          # rename user
usermod -L alice                    # lock account
usermod -U alice                    # unlock account
```

### `userdel` — Delete User
```bash
userdel alice                       # delete user (keep home dir)
userdel -r alice                    # delete user and home directory
```

### `passwd` — Manage Passwords
```bash
passwd                              # change your own password
passwd alice                        # change alice's password (root)
passwd -l alice                     # lock account
passwd -u alice                     # unlock account
passwd -e alice                     # expire (force change at next login)
```

### `groupadd` / `groupdel` — Groups
```bash
groupadd developers
groupdel developers
groups alice                        # list groups for user
```

### `su` / `sudo` — Switch User / Superuser
```bash
su alice                            # switch to alice
su -                                # switch to root with root's environment
su - alice                          # switch to alice with alice's environment
sudo command                        # run command as root
sudo -u alice command               # run command as alice
sudo -i                             # interactive root shell
sudo !!                             # re-run last command with sudo
sudo -l                             # list sudo privileges
visudo                              # safely edit /etc/sudoers
```

### `chmod` — Change Permissions
```bash
# Symbolic notation
chmod u+x file.sh           # add execute for owner
chmod g-w file.txt          # remove write for group
chmod o=r file.txt          # set other to read-only
chmod a+r file.txt          # add read for all
chmod u+x,g-w file.txt      # multiple changes

# Octal notation
chmod 755 script.sh         # rwxr-xr-x
chmod 644 file.txt          # rw-r--r--
chmod 600 private.key       # rw-------
chmod 777 public.sh         # rwxrwxrwx (avoid!)
chmod -R 755 directory/     # recursive

# Numeric reference
# 4=read, 2=write, 1=execute
# 7=rwx, 6=rw-, 5=r-x, 4=r--, 0=---
```

### `chown` — Change Ownership
```bash
chown alice file.txt
chown alice:developers file.txt     # user and group
chown :developers file.txt          # group only
chown -R alice:alice directory/     # recursive
chown --reference=ref.txt target.txt # copy ownership from ref
```

### `chgrp` — Change Group
```bash
chgrp developers file.txt
chgrp -R developers directory/
```

### `umask` — Default Permission Mask
```bash
umask                               # show current mask (e.g., 0022)
umask 027                           # set mask: new files = 640, dirs = 750
```

### `getfacl` / `setfacl` — Access Control Lists
```bash
getfacl file.txt                    # show ACLs
setfacl -m u:alice:rwx file.txt     # grant alice rwx
setfacl -m g:dev:rx directory/      # grant dev group rx
setfacl -x u:alice file.txt         # remove alice's ACL
setfacl -b file.txt                 # remove all ACLs
setfacl -R -m u:alice:rwx dir/      # recursive
```

### `lsattr` / `chattr` — File Attributes
```bash
lsattr file.txt                     # show attributes
chattr +i file.txt                  # immutable (even root cannot delete)
chattr -i file.txt                  # remove immutable flag
chattr +a logfile.txt               # append-only
```

---

## 7. Process Management

### `ps` — Process Snapshot
```bash
ps aux                              # all processes, full format
ps -ef                              # all processes, alternative format
ps aux | grep nginx                 # find a specific process
ps --sort=-%mem | head -10          # top 10 by memory
ps --sort=-%cpu | head -10          # top 10 by CPU
ps -p 1234                          # info about specific PID
ps -u alice                         # processes owned by alice
ps -o pid,ppid,cmd,%mem,%cpu        # custom columns
```

### `top` — Dynamic Process Monitor
```bash
top                                 # interactive view
top -u alice                        # filter by user
top -d 2                            # update every 2 seconds
top -n 1 -b                         # batch mode, single run (great for scripts)
# Inside top: k=kill, r=renice, q=quit, h=help, M=sort by memory, P=sort by CPU
```

### `htop` — Interactive Process Viewer
```bash
htop                                # colorful, mouse-friendly
htop -u alice                       # filter by user
htop -d 5                           # 0.5s delay (value is tenths of seconds)
# F5=tree view, F9=kill, F6=sort, F3=search
```

### `kill` — Send Signal to Process
```bash
kill 1234                           # SIGTERM (graceful)
kill -9 1234                        # SIGKILL (forced)
kill -15 1234                       # SIGTERM (explicit)
kill -HUP 1234                      # SIGHUP (reload config)
kill -l                             # list all signals
```

### `killall` — Kill by Name
```bash
killall nginx
killall -9 firefox
killall -u alice                    # kill all of alice's processes
```

### `pkill` / `pgrep` — Pattern-Based
```bash
pgrep nginx                         # find PIDs of nginx processes
pgrep -u alice                      # PIDs of alice's processes
pkill nginx                         # kill by name
pkill -9 -u alice                   # kill all alice's processes
pkill -f "python script.py"         # match full command line
```

### `nice` / `renice` — Priority
```bash
nice -n 10 command               # run with lower priority (+10 niceness)
nice -n -5 command               # higher priority (needs root for negative)
renice -n 5 -p 1234              # change running process priority
renice -n 5 -u alice             # change all of alice's process priorities
```

### `nohup` — Run Immune to Hangups
```bash
nohup python3 server.py &
nohup python3 server.py > output.log 2>&1 &
# Output goes to nohup.out by default
```

### `jobs` / `bg` / `fg` — Job Control
```bash
jobs                                # list background/stopped jobs
jobs -l                             # with PIDs
bg %1                               # send job 1 to background
fg %1                               # bring job 1 to foreground
Ctrl+Z                              # suspend current foreground job
Ctrl+C                              # kill foreground job
```

### `wait` — Wait for a Process
```bash
wait 1234                           # wait for PID 1234 to finish
wait                                # wait for all background jobs
```

### `screen` — Terminal Multiplexer
```bash
screen                              # start new session
screen -S mysession                 # named session
screen -ls                          # list sessions
screen -r mysession                 # reattach
# Inside: Ctrl+A d=detach, Ctrl+A c=new window, Ctrl+A n=next window
```

### `tmux` — Modern Terminal Multiplexer
```bash
tmux                                # start
tmux new -s work                    # named session
tmux ls                             # list sessions
tmux attach -t work                 # reattach
# Inside: Ctrl+B d=detach, Ctrl+B c=new window, Ctrl+B %=split vertically
```

---

## 8. Disk & Storage

### `df` — Disk Free
```bash
df -h                               # human-readable
df -hT                              # include filesystem type
df -i                               # inode usage
df -h /home                         # specific mount point
```

### `du` — Disk Usage
```bash
du -sh directory/                   # summary, human-readable
du -sh *                            # size of everything in current dir
du -h --max-depth=1                 # one level deep
du -ah                              # all files
du -sh --exclude='.git' .           # exclude a directory
du -k | sort -n | tail -20          # top 20 largest (in KB)
```

### `lsblk` — List Block Devices
```bash
lsblk
lsblk -f                            # include filesystem type and UUID
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT
```

### `fdisk` — Partition Editor
```bash
sudo fdisk -l                       # list all partitions
sudo fdisk /dev/sdb                 # interactive partition editor
# Inside: n=new, d=delete, p=print, w=write, q=quit
```

### `parted` — Advanced Partition Tool
```bash
sudo parted /dev/sdb print          # show partition table
sudo parted /dev/sdb mklabel gpt    # create GPT table
sudo parted /dev/sdb mkpart primary ext4 1MiB 100%
```

### `mkfs` — Make Filesystem
```bash
sudo mkfs.ext4 /dev/sdb1            # format as ext4
sudo mkfs.xfs /dev/sdb1             # format as XFS
sudo mkfs.vfat /dev/sdb1            # format as FAT32
```

### `mount` / `umount` — Mount/Unmount
```bash
mount                               # list all mounts
mount /dev/sdb1 /mnt/data           # mount device
mount -t ext4 /dev/sdb1 /mnt/data   # specify type
mount -o ro /dev/sdb1 /mnt/data     # read-only
mount -o remount,rw /               # remount root read-write
umount /mnt/data                    # unmount
umount -l /mnt/data                 # lazy unmount (when busy)
```

### `/etc/fstab` — Persistent Mounts
```bash
cat /etc/fstab
# UUID=xxxx /data ext4 defaults 0 2
# Device Mountpoint Type Options DumpFreq FsckOrder
```

### `lvm` — Logical Volume Management
```bash
pvdisplay                           # physical volumes
vgdisplay                           # volume groups
lvdisplay                           # logical volumes
pvcreate /dev/sdb                   # create physical volume
vgcreate myvg /dev/sdb              # create volume group
lvcreate -L 20G -n mylv myvg        # create 20G logical volume
mkfs.ext4 /dev/myvg/mylv            # format it
lvextend -L +10G /dev/myvg/mylv     # extend by 10G
resize2fs /dev/myvg/mylv            # resize filesystem after extend
```

### `smartctl` — Disk Health
```bash
sudo smartctl -a /dev/sda           # full drive info
sudo smartctl -H /dev/sda           # health check only
sudo smartctl -t short /dev/sda     # run short test
```

### `dd` — Raw Data Copy
```bash
dd if=/dev/sda of=/dev/sdb bs=4M status=progress   # clone a disk
dd if=/dev/zero of=test.img bs=1M count=1024        # create 1GB file
dd if=/dev/urandom of=data.bin bs=1M count=100      # random data
dd if=ubuntu.iso of=/dev/sdb bs=4M status=progress  # write ISO to USB
```

---

## 9. Networking

### `ip` — Network Configuration (Modern)
```bash
ip addr show                        # all interfaces and IPs
ip addr show eth0                   # specific interface
ip link show                        # link-layer info
ip route show                       # routing table
ip route add default via 192.168.1.1 # add default gateway
ip route del 10.0.0.0/8 via 192.168.1.254
ip neigh show                       # ARP cache
ip addr add 192.168.1.100/24 dev eth0  # add IP
ip link set eth0 up/down            # bring interface up/down
```

### `ping` — Test Connectivity
```bash
ping google.com                     # continuous ping
ping -c 4 google.com                # send 4 packets
ping -i 0.2 google.com              # 0.2s interval
ping -s 1400 google.com             # larger packet size
ping6 google.com                    # IPv6
```

### `traceroute` / `tracepath` — Trace Route
```bash
traceroute google.com
traceroute -n google.com            # no DNS resolution
tracepath google.com                # simpler alternative (no root needed)
```

### `ss` — Socket Statistics (Modern)
```bash
ss -tulpn                           # TCP+UDP listening sockets with PIDs
ss -tulpn | grep :80                # filter by port
ss -s                               # summary statistics
ss -t state established             # established TCP connections
ss -tp                              # TCP with process info
```

### `netstat` — Network Statistics (Legacy)
```bash
netstat -tulpn                      # listening sockets
netstat -an                         # all connections
netstat -rn                         # routing table
```

### `nmap` — Network Scanner
```bash
nmap 192.168.1.1                    # scan host
nmap 192.168.1.0/24                 # scan subnet
nmap -p 80,443 192.168.1.1          # specific ports
nmap -sV 192.168.1.1                # service version detection
nmap -O 192.168.1.1                 # OS detection (root)
nmap -A 192.168.1.1                 # aggressive scan
nmap -p- 192.168.1.1                # all 65535 ports
```

### `curl` — Transfer Data
```bash
curl https://example.com                         # GET request
curl -o file.html https://example.com            # save to file
curl -O https://example.com/file.zip             # save with remote filename
curl -L https://example.com                      # follow redirects
curl -I https://example.com                      # headers only
curl -X POST https://api.example.com/data \
  -H "Content-Type: application/json" \
  -d '{"key":"value"}'                           # POST with JSON
curl -u user:pass https://example.com            # basic auth
curl --cert client.crt --key client.key https:// # mTLS
curl -x http://proxy:8080 https://example.com    # through proxy
curl -# -o file.zip https://example.com/file.zip # progress bar
```

### `wget` — Download Files
```bash
wget https://example.com/file.zip               # download file
wget -O output.zip https://example.com/file.zip # save as
wget -c https://example.com/file.zip            # resume interrupted download
wget -r -np https://example.com/docs/           # mirror directory
wget -q https://example.com                     # quiet mode
wget --limit-rate=500k https://example.com/file.zip # limit speed
```

### `dig` — DNS Lookup
```bash
dig google.com                      # A record
dig google.com MX                   # mail records
dig google.com NS                   # name servers
dig google.com ANY                  # all record types
dig +short google.com               # just the IP
dig @8.8.8.8 google.com             # query specific DNS server
dig -x 8.8.8.8                      # reverse DNS lookup
dig +trace google.com               # trace full DNS resolution
```

### `host` / `nslookup` — DNS Tools
```bash
host google.com
host -t MX google.com
nslookup google.com
nslookup google.com 8.8.8.8         # specific server
```

### `iptables` — Firewall Rules (IPv4)
```bash
iptables -L -n -v                   # list all rules
iptables -A INPUT -p tcp --dport 22 -j ACCEPT    # allow SSH
iptables -A INPUT -p tcp --dport 80 -j ACCEPT    # allow HTTP
iptables -A INPUT -j DROP                         # drop all else
iptables -D INPUT 3                               # delete rule #3
iptables-save > /etc/iptables/rules.v4            # save rules
iptables-restore < /etc/iptables/rules.v4         # restore
```

### `nftables` — Modern Firewall
```bash
nft list ruleset                    # show all rules
nft add table inet filter
nft add chain inet filter input { type filter hook input priority 0\; }
nft add rule inet filter input tcp dport 22 accept
```

### `tcpdump` — Packet Capture
```bash
sudo tcpdump -i eth0                        # capture on interface
sudo tcpdump -i eth0 port 80               # filter by port
sudo tcpdump -i eth0 host 192.168.1.100    # filter by host
sudo tcpdump -i eth0 -w capture.pcap       # save to file
sudo tcpdump -r capture.pcap               # read file
sudo tcpdump -i eth0 -n -c 100             # 100 packets, no DNS
```

### `nc` (netcat) — Network Swiss Army Knife
```bash
nc -l -p 8080                       # listen on port 8080
nc 192.168.1.100 8080               # connect to host:port
nc -zv 192.168.1.100 20-25          # port scan
echo "hello" | nc 192.168.1.100 8080 # send data
nc -l -p 9999 > received.file       # receive file
cat file.txt | nc 192.168.1.100 9999 # send file
```

---

## 10. Package Management

### Debian/Ubuntu (APT)
```bash
apt update                          # refresh package index
apt upgrade                         # upgrade installed packages
apt full-upgrade                    # upgrade with dependency changes
apt install nginx                   # install
apt install nginx=1.18.0            # specific version
apt remove nginx                    # remove (keep config)
apt purge nginx                     # remove + config
apt autoremove                      # remove unused dependencies
apt search nginx                    # search packages
apt show nginx                      # package info
apt list --installed                # list installed packages
apt list --upgradable               # list upgradable packages
dpkg -i package.deb                 # install local .deb file
dpkg -l                             # list installed
dpkg -l | grep nginx                # find installed package
dpkg --get-selections               # all packages
```

### RHEL/CentOS/Fedora (DNF/YUM)
```bash
dnf update                          # update all packages
dnf install nginx                   # install
dnf remove nginx                    # remove
dnf search nginx                    # search
dnf info nginx                      # package info
dnf list installed                  # list installed
dnf history                         # transaction history
dnf history undo 5                  # undo transaction 5
rpm -ivh package.rpm                # install local .rpm
rpm -qa                             # list all installed
rpm -qi nginx                       # package info
rpm -ql nginx                       # list files in package
rpm -qf /etc/nginx/nginx.conf       # which package owns file
```

### Arch Linux (Pacman)
```bash
pacman -Syu                         # sync and upgrade all
pacman -S nginx                     # install
pacman -R nginx                     # remove
pacman -Rs nginx                    # remove with unused deps
pacman -Ss nginx                    # search
pacman -Si nginx                    # info (remote)
pacman -Qi nginx                    # info (installed)
pacman -Ql nginx                    # list files
pacman -Qo /usr/bin/nginx           # which package owns file
```

### Snap / Flatpak / AppImage
```bash
snap install code --classic         # install VS Code via snap
snap list                           # list snaps
snap refresh                        # update snaps

flatpak install flathub org.gimp.GIMP
flatpak list
flatpak update

chmod +x app.AppImage && ./app.AppImage   # run AppImage
```

---

## 11. System Information

### `uname` — Kernel Info
```bash
uname -a                            # all info
uname -r                            # kernel release
uname -m                            # machine hardware (x86_64, aarch64)
uname -s                            # kernel name
uname -n                            # hostname
```

### `hostnamectl` — Hostname and OS
```bash
hostnamectl                         # system info
hostnamectl set-hostname myserver   # change hostname
```

### `lscpu` — CPU Info
```bash
lscpu
lscpu | grep -E "Core|Thread|Socket|MHz"
cat /proc/cpuinfo                   # raw CPU info
```

### `lsmem` — Memory Info
```bash
lsmem
free -h                             # RAM and swap usage
free -h -s 2                        # update every 2 seconds
cat /proc/meminfo                   # detailed memory info
```

### `lshw` — Hardware Summary
```bash
sudo lshw                           # detailed hardware
sudo lshw -short                    # summary table
sudo lshw -class network            # just network devices
sudo lshw -class disk               # just disk devices
lspci                               # PCI devices
lsusb                               # USB devices
lspci -v                            # verbose PCI
```

### `dmidecode` — BIOS/UEFI/Hardware Tables
```bash
sudo dmidecode                      # full DMI info
sudo dmidecode -t bios              # BIOS info
sudo dmidecode -t system            # system info
sudo dmidecode -t memory            # RAM slots and sizes
```

### `uptime` / `w`
```bash
uptime                              # uptime, load average
uptime -p                           # pretty format
w                                   # logged-in users and their activity
who                                 # logged-in users
last                                # login history
last -n 20                          # last 20 logins
lastlog                             # last login per user
```

### `/proc` Filesystem
```bash
cat /proc/version                   # kernel version
cat /proc/loadavg                   # load averages
cat /proc/uptime                    # uptime in seconds
cat /proc/partitions                # disk partitions
cat /proc/mounts                    # current mounts
cat /proc/net/dev                   # network stats
cat /proc/1/status                  # status of PID 1
ls /proc/1234/fd                    # open file descriptors of a process
```

---

## 12. Services & Systemd

### `systemctl` — Service Control
```bash
systemctl status nginx              # status
systemctl start nginx               # start
systemctl stop nginx                # stop
systemctl restart nginx             # restart
systemctl reload nginx              # reload config (if supported)
systemctl enable nginx              # enable at boot
systemctl disable nginx             # disable at boot
systemctl enable --now nginx        # enable and start immediately
systemctl is-active nginx           # check if running
systemctl is-enabled nginx          # check if enabled
systemctl list-units --type=service  # list all services
systemctl list-units --failed        # list failed units
systemctl daemon-reload              # reload systemd config (after editing unit files)
```

### `journalctl` — System Logs
```bash
journalctl                          # all logs (newest last)
journalctl -f                       # follow live
journalctl -n 50                    # last 50 lines
journalctl -u nginx                 # logs for nginx
journalctl -u nginx -f              # follow nginx logs
journalctl -p err                   # errors only (emerg alert crit err warning notice info debug)
journalctl --since "2024-01-01"     # since date
journalctl --since "1 hour ago"
journalctl --until "2024-01-02 12:00:00"
journalctl -b                       # current boot
journalctl -b -1                    # previous boot
journalctl --list-boots             # list boots
journalctl --disk-usage             # log size on disk
journalctl --vacuum-time=7d         # delete logs older than 7 days
journalctl -o json-pretty           # JSON output
journalctl -k                       # kernel messages only
```

### Writing Systemd Unit Files
```ini
# /etc/systemd/system/myapp.service
[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
User=myapp
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/python3 /opt/myapp/server.py
Restart=always
RestartSec=10
StandardOutput=journal
StandardError=journal
Environment=PORT=8080

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now myapp
```

---

## 13. Archiving & Compression

### `tar` — Tape Archive
```bash
# Create
tar -czvf archive.tar.gz directory/          # gzip
tar -cjvf archive.tar.bz2 directory/         # bzip2
tar -cJvf archive.tar.xz directory/          # xz
tar -cvf archive.tar directory/              # no compression

# Extract
tar -xzvf archive.tar.gz                     # extract gzip
tar -xjvf archive.tar.bz2                    # extract bzip2
tar -xJvf archive.tar.xz                     # extract xz
tar -xvf archive.tar -C /tmp/               # extract to /tmp

# Inspect
tar -tzvf archive.tar.gz                     # list contents
tar -xzvf archive.tar.gz specific/file.txt  # extract single file

# Options
# c=create, x=extract, t=list, v=verbose, f=file, z=gzip, j=bzip2, J=xz
```

### `gzip` / `gunzip`
```bash
gzip file.txt                       # compress (replaces file with file.txt.gz)
gzip -k file.txt                    # keep original
gzip -d file.txt.gz                 # decompress (same as gunzip)
gunzip file.txt.gz
gzip -9 file.txt                    # maximum compression
gzip -1 file.txt                    # fastest compression
gzip -l archive.gz                  # list contents
zcat file.txt.gz                    # view compressed file without extracting
```

### `bzip2` / `xz`
```bash
bzip2 file.txt                      # compress
bunzip2 file.txt.bz2                # decompress
bzcat file.txt.bz2                  # view without extracting

xz file.txt                         # compress (best ratio)
xz -d file.txt.xz                   # decompress
xz -k file.txt                      # keep original
xz -9 file.txt                      # maximum compression
xzcat file.txt.xz                   # view without extracting
```

### `zip` / `unzip`
```bash
zip archive.zip file1 file2         # create zip
zip -r archive.zip directory/        # recursive
zip -9 archive.zip file.txt         # max compression
zip -e archive.zip file.txt         # with password
unzip archive.zip                   # extract
unzip archive.zip -d /tmp/          # extract to directory
unzip -l archive.zip                # list contents
unzip -o archive.zip                # overwrite without asking
unzip -p archive.zip file.txt       # extract to stdout
```

### `7z` — 7-Zip
```bash
7z a archive.7z directory/          # create
7z x archive.7z                     # extract
7z l archive.7z                     # list
7z e archive.7z -o/tmp/             # extract to directory
```

---

## 14. SSH & Remote Access

### `ssh` — Secure Shell
```bash
ssh user@hostname
ssh user@192.168.1.100
ssh -p 2222 user@hostname            # custom port
ssh -i ~/.ssh/id_rsa user@hostname   # specific key
ssh -X user@hostname                 # enable X11 forwarding
ssh -L 8080:localhost:80 user@host   # local port forward
ssh -R 9090:localhost:3000 user@host # remote port forward
ssh -D 1080 user@host                # SOCKS5 proxy
ssh -N -f user@host -L 8080:...     # background tunnel (no shell)
ssh -J jumphost user@target          # jump through bastion host
ssh -o StrictHostKeyChecking=no user@host  # skip host key check
```

### SSH Config File (`~/.ssh/config`)
```
Host myserver
    HostName 192.168.1.100
    User alice
    Port 2222
    IdentityFile ~/.ssh/id_ed25519
    ForwardAgent yes

Host bastion
    HostName bastion.example.com
    User ec2-user

Host internal
    HostName 10.0.0.50
    User admin
    ProxyJump bastion
```

### SSH Key Management
```bash
ssh-keygen -t ed25519 -C "email@example.com"   # generate key (ed25519 recommended)
ssh-keygen -t rsa -b 4096                        # RSA 4096-bit
ssh-copy-id user@hostname                        # copy public key to server
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@host  # specific key
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys  # manual copy

ssh-keygen -l -f ~/.ssh/id_ed25519.pub          # fingerprint
ssh-keygen -R hostname                           # remove known host
ssh-agent bash                                   # start agent
ssh-add ~/.ssh/id_ed25519                        # add key to agent
ssh-add -l                                       # list loaded keys
```

### `scp` — Secure Copy
```bash
scp file.txt user@host:/remote/path/             # upload
scp user@host:/remote/file.txt ./local/          # download
scp -r directory/ user@host:/remote/             # recursive
scp -P 2222 file.txt user@host:/remote/          # custom port
```

### `rsync` — Efficient File Sync
```bash
rsync -avz source/ user@host:/destination/       # sync to remote
rsync -avz user@host:/source/ ./destination/     # sync from remote
rsync -avz --delete source/ dest/                # mirror (delete extras)
rsync -avz --exclude='*.log' source/ dest/       # exclude pattern
rsync -avz --progress source/ dest/              # show progress
rsync -avzn source/ dest/                        # dry run (--dry-run)
rsync -avz --backup --backup-dir=/backup source/ dest/  # with backup
rsync -e 'ssh -p 2222' source/ user@host:/dest/  # custom SSH port
```

---

## 15. Scheduling (cron & at)

### `cron` — Recurring Jobs
```bash
crontab -e                          # edit user's crontab
crontab -l                          # list crontab
crontab -r                          # remove crontab
crontab -l -u alice                 # list alice's crontab (root)
sudo crontab -e                     # root's crontab

# System-wide cron directories
ls /etc/cron.daily/
ls /etc/cron.weekly/
ls /etc/cron.hourly/
ls /etc/cron.monthly/
```

Crontab format:
```
# ┌─────────── minute (0-59)
# │ ┌───────── hour (0-23)
# │ │ ┌─────── day of month (1-31)
# │ │ │ ┌───── month (1-12)
# │ │ │ │ ┌─── day of week (0-7, 0=Sunday=7)
# │ │ │ │ │
# * * * * * command

  0 * * * * /usr/bin/python3 /opt/hourly.py       # every hour at :00
  0 2 * * * /usr/bin/backup.sh                     # daily at 2:00 AM
  0 2 * * 0 /usr/bin/weekly.sh                     # every Sunday at 2 AM
  */5 * * * * /usr/bin/check.sh                    # every 5 minutes
  30 8 1 * * /usr/bin/monthly.sh                   # 1st of month 8:30 AM
  @reboot /usr/bin/startup.sh                      # at boot
  @daily /usr/bin/daily.sh                         # shorthand for 0 0 * * *
```

### `at` — One-Time Jobs
```bash
at now + 1 hour                     # schedule for 1 hour from now
at 3:00 PM tomorrow                 # specific time
at 2024-01-15 09:00                 # specific datetime
atq                                 # list pending jobs
atrm 3                              # remove job #3
```

---

## 16. Environment & Shell

### Variables
```bash
echo $HOME                          # home directory
echo $PATH                          # executable search path
echo $USER                          # current username
echo $SHELL                         # current shell
echo $PWD                           # current directory
echo $OLDPWD                        # previous directory
echo $HOSTNAME                      # machine hostname
echo $RANDOM                        # random number (0-32767)
echo $LINENO                        # current line number
echo $$                             # current shell's PID
echo $!                             # last background PID
echo $?                             # last command exit code
```

### `export` / `env` / `printenv`
```bash
export MYVAR="value"                # export to environment
export -n MYVAR                     # unexport
env                                 # print all environment variables
env VAR=val command                 # run command with extra var
printenv HOME                       # print specific variable
printenv                            # all variables (same as env)
unset MYVAR                         # delete variable
```

### `alias`
```bash
alias ll='ls -la'
alias update='sudo apt update && sudo apt upgrade'
alias ports='ss -tulpn'
alias myip='curl -s ifconfig.me'
unalias ll
alias                               # list all aliases
```

### `source` / `.`
```bash
source ~/.bashrc                    # reload shell config
. ~/.bashrc                         # equivalent dot command
source /etc/profile.d/myenv.sh
```

### `history`
```bash
history                             # command history
history 20                          # last 20 commands
history -c                          # clear history
!500                                # re-run command #500
!!                                  # re-run last command
!ssh                                # re-run last command starting with ssh
Ctrl+R                              # reverse search history
```

---

## 17. Hardware & Devices

### `lspci` — PCI Devices
```bash
lspci                               # list all PCI devices
lspci -v                            # verbose
lspci | grep -i vga                 # find GPU
lspci | grep -i ethernet            # find NIC
```

### `lsusb` — USB Devices
```bash
lsusb                               # list USB devices
lsusb -v                            # verbose
lsusb -t                            # tree view
```

### `dmesg` — Kernel Ring Buffer
```bash
dmesg                               # kernel messages
dmesg -H                            # human-readable timestamps
dmesg -w                            # follow (like tail -f)
dmesg | grep -i error               # find errors
dmesg | grep -i usb                 # USB-related messages
dmesg | tail -20                    # last 20 messages
```

### `hdparm` — Hard Disk Performance
```bash
sudo hdparm -I /dev/sda             # drive info
sudo hdparm -tT /dev/sda            # benchmark read speed
```

### `sensors` — Hardware Temperature
```bash
sensors                             # CPU/board temps and fan speeds
watch -n 2 sensors                  # continuous update every 2s
```

---

## 18. Kernel & Modules

### `uname` — Kernel Version
```bash
uname -r                            # kernel release
uname -v                            # kernel version
uname -a                            # everything
```

### `lsmod` — Loaded Modules
```bash
lsmod                               # list all loaded kernel modules
lsmod | grep nvidia                 # check for specific module
```

### `modprobe` / `insmod` / `rmmod`
```bash
sudo modprobe vboxdrv               # load module with dependencies
sudo modprobe -r vboxdrv            # remove module
sudo insmod module.ko               # insert module directly
sudo rmmod module                   # remove module
modinfo nvme                        # module information
```

### `sysctl` — Kernel Parameters
```bash
sysctl -a                           # all parameters
sysctl net.ipv4.ip_forward          # read specific value
sudo sysctl -w net.ipv4.ip_forward=1 # set value (temporary)
# Permanent: add to /etc/sysctl.conf then run:
sudo sysctl -p                      # reload /etc/sysctl.conf
```

Common sysctl settings:
```bash
# Enable IP forwarding (for routing/NAT)
net.ipv4.ip_forward = 1

# Increase file descriptor limits
fs.file-max = 100000

# TCP performance tuning
net.core.somaxconn = 65535
net.ipv4.tcp_max_syn_backlog = 65535
```

---

## 19. Logging

### `syslog` / `rsyslog`
```bash
cat /var/log/syslog                 # general system log
cat /var/log/auth.log               # authentication events
cat /var/log/kern.log               # kernel messages
cat /var/log/dpkg.log               # package installations
cat /var/log/nginx/access.log       # nginx access log
cat /var/log/nginx/error.log        # nginx error log
tail -f /var/log/syslog             # follow system log
grep "error" /var/log/syslog        # search log
```

### `logger` — Write to Syslog
```bash
logger "Manual log entry"
logger -t myapp "Application started"
logger -p user.err "An error occurred"
```

### Log Rotation (`logrotate`)
```bash
sudo logrotate -f /etc/logrotate.conf   # force rotation now
cat /etc/logrotate.conf                  # main config
ls /etc/logrotate.d/                     # per-app configs
```

---

## 20. Security & Firewall

### `firewalld` — Dynamic Firewall (RHEL/Fedora)
```bash
sudo firewall-cmd --state               # firewall status
sudo firewall-cmd --list-all            # list all rules
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --remove-port=80/tcp --permanent
sudo firewall-cmd --reload              # apply permanent changes
sudo firewall-cmd --zone=public --list-ports
```

### `ufw` — Uncomplicated Firewall (Ubuntu)
```bash
sudo ufw status                         # status
sudo ufw enable / disable
sudo ufw allow 22                       # allow SSH
sudo ufw allow 80/tcp                   # allow HTTP
sudo ufw deny 23                        # deny telnet
sudo ufw allow from 192.168.1.0/24     # allow from subnet
sudo ufw allow proto tcp from any to any port 443
sudo ufw delete allow 80               # remove rule
sudo ufw status numbered               # numbered rules
sudo ufw delete 3                      # delete rule #3
sudo ufw reset                         # reset all rules
sudo ufw logging on                    # enable logging
```

### `fail2ban`
```bash
sudo fail2ban-client status                  # overall status
sudo fail2ban-client status sshd             # SSH jail status
sudo fail2ban-client set sshd unbanip IP     # unban an IP
sudo fail2ban-client reload                  # reload config
```

### `openssl` — Cryptography Toolkit
```bash
openssl genrsa -out private.key 4096                   # generate RSA key
openssl req -new -key private.key -out cert.csr        # create CSR
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout key.pem -out cert.pem                        # self-signed cert
openssl s_client -connect example.com:443              # test TLS connection
openssl x509 -in cert.pem -text -noout                 # inspect certificate
openssl x509 -in cert.pem -noout -dates               # check expiry
openssl dgst -sha256 file.txt                          # hash a file
openssl enc -aes-256-cbc -in file.txt -out file.enc    # encrypt
openssl enc -d -aes-256-cbc -in file.enc -out file.txt # decrypt
```

### `gpg` — GNU Privacy Guard
```bash
gpg --gen-key                           # generate key pair
gpg --list-keys                         # list public keys
gpg --list-secret-keys                  # list private keys
gpg -e -r recipient@example.com file.txt   # encrypt for recipient
gpg -d file.txt.gpg                     # decrypt
gpg --sign file.txt                     # sign
gpg --verify file.txt.gpg              # verify signature
gpg --export -a "Name" > public.key     # export public key
gpg --import public.key                 # import public key
```

### `auditd` — Linux Audit System
```bash
auditctl -l                             # list audit rules
auditctl -w /etc/passwd -p rwxa -k passwd_changes   # watch file
ausearch -k passwd_changes              # search audit log by key
aureport --failed                       # failed events report
```

---

## 21. Performance Monitoring

### `vmstat` — Virtual Memory Statistics
```bash
vmstat 2 10                         # every 2s, 10 times
vmstat -s                           # summary statistics
vmstat -d                           # disk statistics
```

### `iostat` — I/O Statistics
```bash
iostat                              # one-time snapshot
iostat 2                            # every 2 seconds
iostat -x                           # extended (with utilization %)
iostat -x 2 5                       # extended, every 2s, 5 times
iostat -d sda                       # specific device only
```

### `sar` — System Activity Reporter
```bash
sar 1 5                             # CPU every 1s, 5 times
sar -r 1 5                          # memory
sar -d 1 5                          # disk
sar -n DEV 1 5                      # network by interface
sar -A                              # all metrics
sar -f /var/log/sysstat/sa01        # from saved log file
```

### `iftop` / `nethogs` — Network Usage
```bash
sudo iftop -i eth0                  # bandwidth per connection
sudo nethogs eth0                   # bandwidth per process
```

### `watch` — Repeat a Command
```bash
watch -n 1 'df -h'                  # disk usage every second
watch -n 2 'ps aux | grep nginx'    # watch a process
watch -d 'cat /proc/loadavg'        # highlight differences
```

### `time` — Measure Command Duration
```bash
time ls -la
time python3 script.py
# Outputs: real (wall clock), user (CPU user), sys (CPU system)
```

### `perf` — Performance Analysis
```bash
sudo perf stat command               # stats (instructions, cache, etc.)
sudo perf top                        # top CPU consumers in real time
sudo perf record -g command          # record with call graphs
sudo perf report                     # display recorded data
```

---

## 22. Miscellaneous Utilities

### `date` / `timedatectl`
```bash
date                                 # current date and time
date "+%Y-%m-%d %H:%M:%S"           # formatted
date -d "yesterday"                  # relative dates
date -d "+3 days"
timedatectl                          # time, timezone, NTP status
timedatectl set-timezone America/New_York
timedatectl set-ntp true
```

### `cal` — Calendar
```bash
cal                                  # current month
cal 2024                             # entire year
cal 3 2024                           # specific month and year
```

### `bc` — Calculator
```bash
echo "5 * 7" | bc
echo "scale=4; 22/7" | bc           # 4 decimal places
bc -l                                # interactive with math library
```

### `yes` / `seq` / `shuf`
```bash
yes | rm -ri directory/             # auto-answer 'y'
seq 1 10                             # numbers 1 to 10
seq 0 2 20                           # 0 to 20 step 2
seq -w 1 10                          # zero-padded
shuf -i 1-100 -n 5                   # 5 random numbers from 1-100
shuf words.txt                       # shuffle lines of a file
```

### `printf` / `echo`
```bash
echo "Hello World"
echo -n "No newline"
echo -e "Tab:\there"                 # interpret escape sequences
printf "%s is %d years old\n" Alice 30
printf "%-20s %5d\n" filename 12345  # formatted output
```

### `tput` — Terminal Control
```bash
tput cols                            # terminal width
tput lines                           # terminal height
tput bold; echo "Bold"; tput sgr0    # bold text
tput setaf 1; echo "Red"; tput sgr0  # red text
tput clear                           # clear screen
tput cup 10 20                       # move cursor to row 10 col 20
```

### `lsof` — List Open Files
```bash
lsof                                 # all open files
lsof -p 1234                         # files opened by PID 1234
lsof -u alice                        # files opened by user alice
lsof /var/log/syslog                 # who has file open
lsof -i :80                          # processes using port 80
lsof -i TCP                          # all TCP connections
lsof -i TCP:80-443                   # ports 80-443
lsof +D /var/log                     # all files under /var/log
```

### `strace` — Trace System Calls
```bash
strace ls                            # trace a command
strace -p 1234                       # attach to running process
strace -e trace=open,read ls        # specific calls only
strace -o trace.log command          # write to file
strace -c ls                         # count and summarize calls
```

### `ltrace` — Trace Library Calls
```bash
ltrace ls                            # trace library calls
```

### `inotifywait` — Watch File Changes
```bash
inotifywait -m -r /var/www          # watch directory recursively
inotifywait -e modify,create /etc    # specific events
```

### `xargs` Advanced
```bash
cat urls.txt | xargs -P5 wget        # download 5 in parallel
find . -name "*.log" -print0 | xargs -0 grep "ERROR"  # handle spaces in filenames
ls | xargs -I{} mv {} /backup/{}     # rename with placeholder
```

### `column` / `pr` — Formatting
```bash
column -t /etc/fstab                 # tabular fstab
pr -2 -l 60 file.txt                 # format in 2 columns
```

### `script` — Record Terminal Session
```bash
script session.log                   # start recording (exit to stop)
scriptreplay timing.log session.log  # replay (requires -t flag when recording)
script -t 2>timing.log session.log   # record with timing
```

### `nl` — Number Lines
```bash
nl file.txt                          # number non-empty lines
nl -b a file.txt                     # number all lines
```

### `rev` / `tac`
```bash
rev file.txt                         # reverse each line character by character
echo "hello" | rev                   # olleh
tac file.txt                         # reverse line order (last line first)
```

### `timeout` — Run with Time Limit
```bash
timeout 30 long_running_command      # kill after 30 seconds
timeout 5m ./script.sh              # 5-minute limit
timeout --kill-after=10 30 command   # send KILL after 10s if still running
```

### `parallel` — Run Commands in Parallel
```bash
parallel echo ::: A B C D                         # parallel echo
cat urls.txt | parallel wget                       # parallel wget
ls *.jpg | parallel convert {} {.}.png             # parallel image conversion
parallel -j4 gzip ::: *.log                       # 4 at a time
```

---

*Last updated: 2026 | This document covers mainstream Linux distributions.*
