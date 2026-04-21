# Linux Notes Guide: Basic to Advanced

## Table of Contents
- [Introduction](#introduction)
- [Terminal Basics](#terminal-basics)
- [File System Hierarchy](#file-system-hierarchy)
- [Essential File and Directory Commands](#essential-file-and-directory-commands)
- [Text Editors: Nano and Vim](#text-editors-nano-and-vim)
- [Permissions and Ownership](#permissions-and-ownership)
- [Pipes and Redirection](#pipes-and-redirection)
- [Process Management](#process-management)
- [Package Management](#package-management)
- [Users, Groups, and Sudo](#users-groups-and-sudo)
- [Shell Scripting Basics](#shell-scripting-basics)
- [Networking Basics and Administration](#networking-basics-and-administration)
- [Cron Jobs and Systemd](#cron-jobs-and-systemd)
- [Disk Management, Partitions, Mounts, and LVM](#disk-management-partitions-mounts-and-lvm)
- [Kernel Modules](#kernel-modules)
- [Security: SELinux, AppArmor, and Firewalls](#security-selinux-apparmor-and-firewalls)
- [Troubleshooting Common Issues](#troubleshooting-common-issues)
- [Quick Reference Cheat Sheets](#quick-reference-cheat-sheets)
- [Practice Challenges](#practice-challenges)

## Introduction
Linux is a Unix-like operating system built around the Linux kernel, typically used with GNU utilities, shells, package managers, and service managers to create complete distributions such as Ubuntu, Debian, Fedora, and Rocky Linux.[1][2] A Linux system is commonly learned through the command line because the shell exposes the file system, processes, permissions, networking tools, and automation features directly.[1][3]

Linux distributions vary in package tools, defaults, and release models, but they share common fundamentals such as the hierarchical file system, POSIX-style permissions, and text-based administration workflows.[1][2] That consistency is why learning core commands and system concepts transfers well across most servers, developer machines, and embedded Linux environments.[1][3]

## Terminal Basics
A shell is a command interpreter that reads user input and runs programs; Bash is one of the most common shells on Linux systems.[1][3] The prompt usually shows the current user, host, and working directory, which helps track where commands will act.[1]

### Core ideas
- `pwd` prints the present working directory.[1]
- `ls` lists files and directories; `ls -l` adds details and `ls -a` includes hidden entries beginning with a dot.[1][3]
- `cd` changes directories; `cd ..` moves up one level and `cd ~` moves to the current user's home directory.[1][3]
- Command history can be reviewed with `history`, and previous commands can be reused with the arrow keys or `!number` expansion in Bash.[3]

### Helpful navigation examples
```bash
pwd
ls -la
cd /etc
cd ..
cd ~
```

### Notes
Use Tab for autocomplete when entering paths or commands.[3] Use `man command` for the manual page and `command --help` for quick syntax help.[1][3]

## File System Hierarchy
Linux uses a single directory tree rooted at `/`, and devices, processes, and many system interfaces are exposed as files or file-like objects.[1][2] Understanding standard directories is essential because configuration, binaries, logs, and user data are intentionally separated.[1][2]

| Directory | Purpose |
|---|---|
| `/` | Root of the entire file system.[1] |
| `/home` | Regular user home directories.[1][2] |
| `/root` | Home directory of the root user.[2] |
| `/etc` | System-wide configuration files.[1][2] |
| `/bin` and `/sbin` | Essential user and system binaries; modern systems may map these into `/usr`.[2] |
| `/usr` | Userland applications, libraries, and shared data.[1][2] |
| `/var` | Variable data such as logs, caches, spool files, and databases.[2] |
| `/tmp` | Temporary files, often cleared periodically.[1] |
| `/dev` | Device files representing hardware and pseudo-devices.[2] |
| `/proc` | Virtual file system exposing kernel and process information.[2] |
| `/sys` | Kernel and device model information.[2] |
| `/mnt` and `/media` | Temporary and removable media mount points.[2] |

### Example checks
```bash
ls /
ls /etc
ls /var/log
cat /proc/cpuinfo
```

## Essential File and Directory Commands
Basic file operations are performed with standard commands that create, copy, move, inspect, and delete files or directories.[1][3] These commands are foundational because administration and scripting rely heavily on predictable file manipulation.[1][3]

### Common commands
- `mkdir dir1` creates a directory; `mkdir -p a/b/c` creates nested directories as needed.[1][3]
- `touch file.txt` creates an empty file if it does not exist, or updates its timestamp if it does.[1]
- `cp src dst` copies files; `cp -r dir1 dir2` copies directories recursively.[1][3]
- `mv old new` moves or renames files and directories.[1]
- `rm file` removes a file; `rm -r dir` removes a directory tree recursively, and `rm -f` forces deletion without prompts.[1][3]
- `find /path -name "*.log"` searches by name; `locate pattern` can search faster if its index is available.[3]

### Examples
```bash
mkdir notes
cd notes
touch linux.txt
cp linux.txt linux_backup.txt
mv linux_backup.txt backup.txt
rm backup.txt
```

### Safe habits
Use `cp -i`, `mv -i`, and `rm -i` when learning to avoid accidental overwrites or deletions.[3] Always verify the current directory with `pwd` before recursive operations such as `rm -r`.[1]

## Text Editors: Nano and Vim
Linux administrators often edit configuration files directly in terminal editors because remote access and rescue modes may not provide a graphical interface.[1][3] Nano is beginner-friendly, while Vim is modal and faster for advanced editing once its modes and commands are learned.[3]

### Nano basics
- Open a file with `nano filename`.[3]
- Save with `Ctrl+O`, confirm the file name, and press Enter.[3]
- Exit with `Ctrl+X`.[3]
- Search with `Ctrl+W`.[3]

### Vim basics
Vim has normal mode for commands, insert mode for typing, and command-line mode for save or quit actions.[3] Press `i` to enter insert mode, `Esc` to return to normal mode, `:w` to save, `:q` to quit, and `:wq` to save and quit.[3]

```bash
nano /tmp/test.txt
vim /tmp/test.txt
```

### Useful Vim motions
- `dd` deletes the current line.[3]
- `yy` yanks a line and `p` pastes it.[3]
- `/word` searches forward for text.[3]
- `:%s/old/new/g` replaces all matches in a file.[3]

## Permissions and Ownership
Linux permissions control which users can read, write, or execute files and directories, and ownership ties each object to a user and a group.[1][2] The `ls -l` format exposes file type, permission bits, owner, group, size, and timestamps in a compact view.[1]

### Permission model
The three permission sets are for user, group, and others, and each set may include read (`r`), write (`w`), and execute (`x`) bits.[1][2] For directories, execute means the ability to traverse into the directory, while write allows creating or deleting entries if directory permissions permit it.[2]

### Core commands
- `chmod` changes permission bits.[1]
- `chown` changes owner and optionally group.[1][2]
- `chgrp` changes group ownership.[2]

### Examples
```bash
ls -l script.sh
chmod +x script.sh
chmod 755 script.sh
chown alice:developers project.txt
chgrp developers shared.txt
```

### Numeric permissions
- `7` = read + write + execute.[1]
- `6` = read + write.[1]
- `5` = read + execute.[1]
- `4` = read only.[1]

### Special bits
Setuid, setgid, and sticky bit modify standard behavior in specific cases such as running with file owner identity, inheriting group ownership, or protecting shared directories like `/tmp` from arbitrary deletion.[2] These are commonly seen in administrative tools and shared workspaces.[2]

## Pipes and Redirection
The shell can redirect standard input, standard output, and standard error so commands can read from files, write to files, or send output into other commands.[1][3] Pipes are powerful because they let small single-purpose tools be chained into larger workflows.[1][3]

### Redirection operators
- `>` writes stdout to a file, replacing existing content.[1]
- `>>` appends stdout to a file.[1]
- `<` uses a file as stdin.[1]
- `2>` redirects stderr.[3]
- `2>&1` combines stderr with stdout.[3]

### Pipe examples
```bash
ls -l | less
dmesg | grep -i usb
cat access.log | grep 404 | wc -l
find /etc -name "*.conf" 2>/dev/null | sort
```

### Frequently combined text tools
- `grep` filters lines matching patterns.[3]
- `sort` sorts lines alphabetically or numerically.[3]
- `uniq` removes adjacent duplicates, often after `sort`.[3]
- `wc` counts lines, words, or bytes.[1]
- `cut`, `tr`, `sed`, and `awk` are useful for extraction and transformation.[3]

## Process Management
Every running program is a process, and Linux provides tools to inspect, prioritize, signal, and terminate them.[1][2] Process control matters for troubleshooting CPU spikes, stuck applications, background jobs, and service failures.[1][3]

### Key commands
- `ps` reports process snapshots; `ps aux` is a common full listing.[1][3]
- `top` shows live process activity, CPU use, memory use, and load.[1][3]
- `kill PID` sends a signal to a process, with `SIGTERM` as the default.[1]
- `kill -9 PID` sends `SIGKILL`, which forcefully stops a process without cleanup.[1][2]
- `jobs`, `bg`, and `fg` manage shell jobs started in the current session.[3]
- `nohup command &` keeps a process running after logout in many cases.[3]

### Examples
```bash
ps aux | grep nginx
top
kill 1234
kill -9 1234
sleep 300 &
jobs
fg %1
```

### Signals to remember
- `SIGTERM` asks a process to terminate cleanly.[2]
- `SIGKILL` forcibly stops a process.[2]
- `SIGHUP` historically indicated terminal hangup and is often used for reload behavior.[2]

## Package Management
Package managers install, update, remove, and verify software from repositories maintained by the distribution.[2][3] Debian-based systems use APT, while Fedora and RHEL-family systems commonly use DNF or YUM, with DNF being the modern default on newer releases.[2][3]

### APT examples
```bash
sudo apt update
sudo apt upgrade
sudo apt install curl
sudo apt remove curl
sudo apt search nginx
```

### DNF examples
```bash
sudo dnf check-update
sudo dnf install httpd
sudo dnf remove httpd
sudo dnf search nginx
```

### YUM examples
```bash
sudo yum install httpd
sudo yum update
sudo yum remove httpd
```

### Notes
Repository metadata should usually be refreshed before large upgrades, and package names differ across distributions even when software is equivalent.[2][3] Logs and package database state are useful when debugging failed installs or dependency issues.[2]

## Users, Groups, and Sudo
Linux is a multi-user operating system, so identity and privilege separation are central to both usability and security.[1][2] User records, group memberships, and password information are stored in standard account databases such as `/etc/passwd`, `/etc/group`, and shadow password storage.[2]

### Useful commands
- `whoami` shows the current username.[1]
- `id` shows UID, GID, and group memberships.[1]
- `useradd`, `usermod`, and `userdel` manage local accounts.[2]
- `groupadd` and `groupdel` manage groups.[2]
- `passwd` changes passwords.[1]
- `sudo` runs a command with elevated privileges according to policy.[2]

### Examples
```bash
whoami
id
sudo useradd -m devuser
sudo passwd devuser
sudo usermod -aG sudo devuser
sudo groupadd project
```

### Important concepts
The root account has unrestricted administrative power, so routine work is often done with an unprivileged user and temporary elevation through `sudo`.[2] Membership changes may require logging out and back in before a new shell session recognizes them.[2]

## Shell Scripting Basics
Shell scripts automate repeatable tasks by combining commands, variables, conditions, loops, and functions inside a text file interpreted by a shell such as Bash.[3] Scripting is one of the fastest ways to improve productivity in Linux because many administrative tasks are repetitive and text-oriented.[1][3]

### Minimal script
```bash
#!/bin/bash
name="Linux"
echo "Hello, $name"
```

Save the script as `hello.sh`, run `chmod +x hello.sh`, and execute it with `./hello.sh`.[3]

### Variables and input
```bash
#!/bin/bash
read -p "Enter your name: " user
echo "Welcome, $user"
```

### Condition example
```bash
#!/bin/bash
if [ -f /etc/passwd ]; then
  echo "passwd file exists"
else
  echo "missing"
fi
```

### Loop example
```bash
#!/bin/bash
for f in *.txt; do
  echo "$f"
done
```

### Best practices
- Start with a shebang such as `#!/bin/bash`.[3]
- Quote variable expansions like `"$var"` unless word splitting is specifically desired.[3]
- Check exit status with `$?` or structure commands with `if` directly.[3]
- Use `bash -x script.sh` for tracing during debugging.[3]

## Networking Basics and Administration
Linux networking tools help inspect interfaces, routes, firewall rules, name resolution, and secure remote access.[2][3] Newer systems prefer the `ip` command suite over legacy tools like `ifconfig`, though both may still appear in documentation and older environments.[2][3]

### Interface and route inspection
```bash
ip a
ip route
ifconfig
ping 8.8.8.8
ss -tulpn
```

### Remote access tools
- `ssh user@host` opens a secure remote shell.[3]
- `scp file user@host:/path/` copies files over SSH.[3]
- `sftp user@host` provides interactive file transfer over SSH.[2]

### Firewall and packet filtering
`iptables` manages packet filtering, NAT, and forwarding rules in many Linux systems, although some distributions now use higher-level front ends or nftables underneath.[2] Basic firewall practice includes allowing only required inbound ports and auditing existing rules before making changes.[2]

```bash
sudo iptables -L -n -v
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
```

### DNS and connectivity checks
Use `ping`, `ss`, `traceroute`, `dig`, and log review to separate name resolution issues from routing or service binding issues.[2][3] A failed SSH connection may be caused by DNS errors, wrong credentials, blocked ports, stopped services, or firewall rules.[2]

## Cron Jobs and Systemd
Linux systems commonly use cron for scheduled recurring commands and systemd for service management, boot targets, and centralized logs on many modern distributions.[2][3] These tools are essential for automation, maintenance, and server operations.[2]

### Cron basics
`crontab -e` edits the current user's scheduled jobs, with each line defining minute, hour, day of month, month, day of week, and command.[3] Cron jobs run with a limited environment, so full paths and explicit environment assumptions are safer than interactive-shell shortcuts.[2][3]

```bash
crontab -e
0 2 * * * /usr/bin/backup.sh
*/15 * * * * /usr/bin/date >> /tmp/time.log
```

### Systemd service management
```bash
sudo systemctl status ssh
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl enable ssh
sudo systemctl disable ssh
journalctl -u ssh --since today
```

### Concepts
A systemd unit may represent a service, socket, mount, timer, or target, and unit files describe how the system should start and supervise them.[2] `journalctl` is commonly used to inspect recent logs for boot problems and service failures on systemd-based systems.[2]

## Disk Management, Partitions, Mounts, and LVM
Disk administration includes discovering block devices, partitioning disks, creating file systems, mounting them, and optionally abstracting storage through LVM for flexible resizing and management.[2] Mistakes in this area can destroy data, so commands should be verified carefully before execution.[2]

### Discovery and partitioning
```bash
lsblk
sudo fdisk -l
sudo fdisk /dev/sdb
```

### File systems and mounts
```bash
sudo mkfs.ext4 /dev/sdb1
sudo mkdir -p /mnt/data
sudo mount /dev/sdb1 /mnt/data
mount | grep sdb1
sudo umount /mnt/data
```

### Persistent mounts
Persistent mounts are usually defined in `/etc/fstab`, often using UUIDs from `blkid` because device names can change across boots.[2] A broken `fstab` entry can delay boot or drop the system into recovery depending on options and the distribution.[2]

### LVM basics
Logical Volume Manager adds a layer between physical storage and mounted file systems so capacity can be pooled and resized more flexibly than fixed partitions.[2] The main objects are physical volumes, volume groups, and logical volumes.[2]

```bash
sudo pvcreate /dev/sdb1
sudo vgcreate data_vg /dev/sdb1
sudo lvcreate -L 5G -n data_lv data_vg
sudo mkfs.ext4 /dev/data_vg/data_lv
sudo mount /dev/data_vg/data_lv /mnt/data
```

### Helpful commands
- `df -h` shows mounted file systems and usage.[1]
- `du -sh *` estimates directory sizes.[1][3]
- `blkid` shows device UUIDs and file system types.[2]
- `vgs`, `lvs`, and `pvs` summarize LVM state.[2]

## Kernel Modules
Kernel modules are pieces of code that can be loaded into or removed from the running kernel to provide drivers, file systems, and other functionality without rebuilding the whole kernel.[2] This modular design lets Linux adapt to hardware and features dynamically.[2]

### Common commands
```bash
lsmod
modinfo e1000
sudo modprobe loop
sudo modprobe -r loop
```

### Concepts
`lsmod` lists loaded modules, `modinfo` shows metadata about a module, and `modprobe` loads a module along with its dependencies when available.[2] Module loading problems can stem from missing drivers, kernel version mismatches, unsigned modules under secure boot policies, or unresolved dependencies.[2]

## Security: SELinux, AppArmor, and Firewalls
Linux security relies on layered controls that include traditional permissions, privilege separation, mandatory access control frameworks, authentication policy, and network filtering.[2] SELinux and AppArmor restrict what processes can do even after they start, reducing damage if a service is compromised.[2]

### SELinux
SELinux is a mandatory access control system widely used in RHEL-family distributions, where policy labels define allowed interactions between subjects and objects.[2] Typical operating modes are enforcing, permissive, and disabled, and context labeling is central to policy decisions.[2]

```bash
getenforce
sestatus
ls -Z /var/www/html
```

### AppArmor
AppArmor is a path-based mandatory access control framework commonly associated with distributions such as Ubuntu.[2] It uses profiles to restrict programs and can run in enforce or complain mode for policy development and debugging.[2]

```bash
sudo aa-status
sudo apparmor_status
```

### Firewalls
Host firewalls reduce exposed attack surface by limiting reachable services and logging unwanted traffic as needed.[2] Systems may use `iptables`, `firewalld`, `ufw`, or nftables-based backends depending on the distribution.[2]

```bash
sudo ufw status
sudo ufw allow 22/tcp
sudo firewall-cmd --list-all
```

### Security habits
- Keep packages updated to reduce known vulnerabilities.[2][3]
- Prefer SSH keys over passwords where practical.[2]
- Avoid direct root login over SSH unless there is a specific operational reason.[2]
- Review logs regularly for failed logins, denials, and unexpected service behavior.[2]

## Troubleshooting Common Issues
Troubleshooting on Linux usually becomes easier when approached in layers: identify symptoms, narrow the scope, inspect logs, verify recent changes, and test one component at a time.[2][3] A methodical approach prevents unnecessary reconfiguration and reduces the risk of making a broken system harder to recover.[2]

### System will not boot normally
- Check boot target, recent package changes, and file system integrity.[2]
- Use recovery mode or emergency shell if available.[2]
- Review logs with `journalctl -xb` on systemd systems.[2]

### Disk is full
- Use `df -h` to identify the full file system and `du -sh` to find large directories.[1][3]
- Inspect log growth in `/var/log`, caches, package artifacts, and old backups.[2]
- Remove safely rather than deleting unknown files blindly.[2]

### Permission denied
- Check ownership and permission bits with `ls -l`.[1]
- Verify directory execute permission for traversal.[2]
- On hardened systems, inspect SELinux or AppArmor denials as well.[2]

### Network not working
- Check link state and addresses with `ip a`.[2][3]
- Confirm routing with `ip route` and test with `ping` or `traceroute`.[2]
- Confirm the service is listening with `ss -tulpn` and not blocked by firewall policy.[2]

### Service fails to start
- Check `systemctl status service` and `journalctl -u service`.[2]
- Validate configuration syntax where the service provides a test mode, such as web servers and SSH daemons.[2]
- Look for port conflicts, missing files, permission issues, and invalid environment variables.[2]

## Quick Reference Cheat Sheets
These condensed command groups are meant for fast recall during hands-on work.[1][3]

### Navigation and files
```bash
pwd
ls -la
cd /path
mkdir -p dir/subdir
touch file.txt
cp -r src dst
mv old new
rm -r dir
find . -name "*.conf"
```

### Viewing and editing
```bash
cat file
less file
head -n 20 file
tail -f file
nano file
vim file
```

### Permissions and ownership
```bash
ls -l
chmod 644 file
chmod 755 script.sh
chown user:group file
chgrp group file
```

### Search and pipelines
```bash
grep -i error /var/log/syslog
sort file.txt | uniq
ps aux | grep ssh
find /etc -name "*.conf" 2>/dev/null | less
```

### Processes and services
```bash
ps aux
top
kill PID
kill -9 PID
systemctl status ssh
journalctl -u ssh --since today
```

### Packages and users
```bash
apt update && apt upgrade
dnf install package
yum update
id
sudo usermod -aG sudo username
passwd username
```

### Networking and storage
```bash
ip a
ss -tulpn
ssh user@host
scp file user@host:/tmp/
df -h
lsblk
mount | grep /mnt
```

## Practice Challenges
Practice is where Linux knowledge becomes usable, so each section below includes tasks that force command recall, interpretation, and safe system habits.[1][3]

### Terminal basics
1. Open a shell, print the current directory, list all files including hidden ones, and move from your home directory to `/etc` and back.[1][3]
2. Use `man ls` and identify three useful options beyond plain `ls`.[1][3]
3. Use history expansion to rerun a previous command without typing it fully.[3]

### File system hierarchy
1. Explore `/etc`, `/var/log`, `/proc`, and `/dev`, then write one sentence describing each path.[2]
2. Find the file that stores user account names and inspect it safely with `less`.[2]
3. Use `cat /proc/cpuinfo` and identify your CPU model line.[2]

### File commands
1. Create a project directory tree with nested folders in one command.[3]
2. Create three files, copy one, rename another, and delete the copy.[1]
3. Use `find` to list every `.txt` file under your working directory.[3]

### Nano and Vim
1. Create one file in Nano and one in Vim, save both, then reopen them for editing.[3]
2. In Vim, delete a line, paste it elsewhere, and replace one repeated word globally.[3]
3. In Nano, search for a word and then save the file under the same name.[3]

### Permissions
1. Create a shell script, make it executable, and confirm the permission change with `ls -l`.[1]
2. Change a file to mode `640` and explain which users can read or write it.[1]
3. Create a shared directory and experiment with owner and group changes in a safe test area.[2]

### Pipes and redirects
1. Count how many lines in a log file contain the word `error` using a pipeline.[3]
2. Redirect command output into a file, append more output, then redirect errors separately.[1][3]
3. Build a command that finds all `.conf` files and sorts the results.[3]

### Processes
1. Start a background `sleep` command, view it with `jobs`, and bring it back to the foreground.[3]
2. Use `ps aux | grep` to find a process by name.[1][3]
3. Compare stopping a process with `kill` and `kill -9` in a test scenario.[2]

### Package management
1. Search for a package, install it, inspect its version, and remove it.[2][3]
2. Run the appropriate metadata refresh and upgrade command for your distribution.[2][3]
3. Record one difference between APT and DNF or YUM syntax.[2][3]

### Users, groups, and sudo
1. Create a test user with a home directory and set a password.[2]
2. Add the user to a supplemental group and verify with `id`.[2]
3. Explain why routine work should not be done as root.[2]

### Shell scripting
1. Write a script that prints the current date and user name.[3]
2. Write a script that checks whether a file exists and prints a message.[3]
3. Write a `for` loop that echoes every `.log` file in the current directory.[3]

### Networking
1. Display your IP addresses and default route.[2][3]
2. Test connectivity to a host by IP and then by domain name.[2]
3. Use `ssh` to connect to a test machine and copy a file with `scp`.[3]

### Cron and systemd
1. Schedule a cron job that writes the date to a file every 15 minutes.[3]
2. Check whether the SSH service is enabled and active.[2]
3. Read the last 50 journal lines for a service of your choice.[2]

### Disk and LVM
1. List block devices and mounted file systems.[2]
2. Mount a test file system to `/mnt/data` and unmount it again.[2]
3. On a lab machine, identify the roles of PV, VG, and LV in LVM.[2]

### Kernel modules and security
1. List loaded kernel modules and inspect metadata for one of them.[2]
2. Check whether SELinux or AppArmor is active on your system.[2]
3. List current firewall rules or status using the firewall tool available on your distribution.[2]

## Suggested Learning Path
A practical sequence is terminal basics, files, editors, permissions, text processing, processes, packages, users, shell scripting, networking, services, storage, and then advanced topics like LVM, kernel modules, and mandatory access control.[1][3] That order works well because each later topic depends on earlier command fluency and confidence with safe system changes.[1][2]
