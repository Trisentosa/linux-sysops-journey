<div align="center">
  <h1>Linux Sysops Learning Journey </h1>
</div>

<h1>Table of Contents</h1>

- [Course](#course)
- [Handbook](#handbook)
  - [Intro and Setup](#intro-and-setup)
    - [Linux Distribution](#linux-distribution)
    - [Setup](#setup)
    - [Snapshots](#snapshots)
  - [The Linux Terminal](#the-linux-terminal)
    - [Terminologies](#terminologies)
    - [Terminator](#terminator)
    - [Linux Command Structure](#linux-command-structure)
    - [Getting Help](#getting-help)
    - [Type](#type)
    - [Mastering the `Tab` key](#mastering-the-tab-key)
    - [Keyboard Shortcuts](#keyboard-shortcuts)
    - [Bash History](#bash-history)
    - [Root Access](#root-access)
  - [Linux File System](#linux-file-system)
    - [Intro](#intro)
    - [Viewing Files](#viewing-files)
    - [More File Operations](#more-file-operations)
    - [Finding Files and String](#finding-files-and-string)
    - [VIM](#vim)
    - [Hard and Symlinks](#hard-and-symlinks)
  - [User Account Management](#user-account-management)
  - [Linux File Permission](#linux-file-permission)
  - [Linux Process Management](#linux-process-management)
    - [Processes and Linux Security Model](#processes-and-linux-security-model)
    - [Listing Processes](#listing-processes)
    - [Signals and Killing Processes](#signals-and-killing-processes)
    - [Foreground vs Bakground Processes](#foreground-vs-bakground-processes)
    - [Job Control](#job-control)
  - [Networking in Linux](#networking-in-linux)
  - [Software Management](#software-management)
  - [System Administration](#system-administration)
  - [Bash Scripting](#bash-scripting)
- [Extras](#extras)
  - [Use AI and Natural Language to Administer Linux Systems (ChatGPT \& ShellGPT)](#use-ai-and-natural-language-to-administer-linux-systems-chatgpt--shellgpt)
  - [Configure a Linux Server from Scratch (VPS,DNS,WEB,PHP,MySQL,Wordpress)](#configure-a-linux-server-from-scratch-vpsdnswebphpmysqlwordpress)
  - [Security: Information Gathering and Sniffing Traffic](#security-information-gathering-and-sniffing-traffic)
  - [IPFS: Interplanetary File System](#ipfs-interplanetary-file-system)
  - [Netfilter and Iptables Firewall](#netfilter-and-iptables-firewall)
  - [Security: SSH Public Key Authentication](#security-ssh-public-key-authentication)
- [Challenges](#challenges)
  - [The Linux Terminal](#the-linux-terminal-1)
  - [Linux File System](#linux-file-system-1)
  - [File Permissions](#file-permissions)
  - [Process Management](#process-management)
  - [Networking on Linux](#networking-on-linux)
  - [Software Management](#software-management-1)
  - [System Administration](#system-administration-1)
  - [Bash Scripting](#bash-scripting-1)
  - [Netfilter and Iptables](#netfilter-and-iptables)
- [Others](#others)

# Course
https://www.udemy.com/course/master-linux-administration/

# Handbook

## Intro and Setup

### Linux Distribution

A Linux Distribution is an Operating System made from a software collection that is based
upon the Linux kernel and, often, a package management system.
The Linux OS comprises:
- the Linux Kernel
- the GNU shell utilities
- the graphical desktop environment and more

Most used Linux Distribution:
- Ubuntu (will be used in this course, but what we will learn will be linux distribution independent)
- Redhat

[Linux Distribution Database](https://distrowatch.com/) 

### Setup

Setup [Vbox](https://www.virtualbox.org/) with [Ubuntu LTS Version](https://ubuntu.com/download/desktop)

#### Updating package
- `sudo apt update` : updates the package lists for upgrades for packages that need upgrading, as well as new packages that have just come to the repositories.
- `sudo apt full-upgrade` : update applications to the latest versions

#### Device Integration
- Devices -> Insert Guest Additions CD Image
- If that doesnt work, go to terminal
  - `cd /media/<username>/Vbox_GA_…`
  - `sudo bash VBoxLinuxAddtions.run`

### Snapshots
Can restore the snapshots using the Virtualbox when the VM is on saved or powered off state. 
Very useful to do so can rollback anytime, especially when making huge changes to your VM
- Machine -> Take Snapshots

## The Linux Terminal

### Terminologies
- **A Terminal Emulator (Terminal)**: crucial part of any Linux system because it basically allows you to access the system through a shell. 
- **Shell**:  is a program that takes commands from the user and gives them to the operating system’s kernel to execute. It’s also called the command interpreter. The shell gets started when the user logs in or starts the terminal. E.g. Bash
- **Console**: special sort of terminal, that is launched when no GUI is installed

### Terminator

Terminal application with extended feature than linux default terminal. 
- Install Terminator: `sudo apt update && sudo apt install terminator`
- More features such as themes, split terminal screen feature, etc

### Linux Command Structure
Structure: 

```
<command-name> -<options> <command arguments>
```

Notes: 
* In some cases options can come after arguments, but the standard way is before arguments.
* White space between command arguments, name, and options can be more than 1 whitespaces
* Single hyphen (-) for abbreviation vs double hyphen (--) for whole word (E.g. ls -a vs ls –all)
* Combining options: for example, instead of `ls -l -h -a`, can do `ls -lha`


### Getting Help

* Getting Help, Man Pages
- `man` command: read documentation of a specific command 
  - E.g. `man ls`
- How to read command usage (Synopsis section)
  -  E.g. `ls [OPTION]... [FILE]...`
     -  `...`(ellipsis) means it can take multiple of a thing
     -  `[]`(square brackets) means that a thing is optional for that command to work
     -  In above example basically can take multiple options and multiple arguments called file
  - Other conventions
    - underline : substitute as something appropriate (e.g --block_size == <u>SIZE</u>)
    - bold : type exactly as it is (e.g. **-a --all**)
- Shortcuts and keys for man page
  - `Ctrl+F` (Go down pages/forward) / `Ctrl+B` (Go up pages/backward)
  - `g` (beginning of page) / `Shift+g`(end of page)
  - `/anything` : will search for matching keyword after the `/` (forward search)
  - `n`: get the next searched match / `shift+n`: get previous match
  - `?anything`: similar to `/` but it will search backwards. Use when you are at the end of pages
- Finding keywords in all man pages:
  - `man -k anykeyword`
    - If the keyword is quoted(“”), it will search for that string in all man pages (e.g. man -k “copy files”)
    - If not quoted, it will find the command and description that match the keyword (e.g. man -k ifconfig). This is the same as using `apropos` (e.g. apropos ifconfig)

### Type 
- `type` command
- We can check where the executable path of a command by doing `type <command-name>`
- If it is a shell builtin command (e.g. `cd`, `umask`). Then we use `help` instead of `man`
- Alternatively, can use `--help` for any command regardless whether that command is built in or not

### Mastering the `Tab` key
For auto completion, if multiple options exist need to press `tab` twice to show all the options. Always use tab completion to avoid making mistakes (as in using tab will indicate if such file/command exist, prevent making semantical error and accidents)

### Keyboard Shortcuts
- `Ctrl + L` : same as `clear` command, clearing the terminal
- `Ctrl + D`: same as `exit` command, close the terminal
- `Ctrl + A`: will go to the beginning of the command, useful when you want to just change the command name 
- `Ctrl + E`: reverse of `Ctrl+A`, go to the end of the line
- `Ctrl + U`: delete whole line before it
- `Ctrl + C`: stop a process
- `Ctrl + Z`: pause a process and can be resumed later (`bg %1`)

### Bash History

- `cat .bash_history`: located in the home directory, shows bash command history, the size of this list is determined by the variable `HISTFILESIZE` (default 2000)
- `history`: shows the executed command in numbered order (current terminal session), the size of this list is determined by the variable `HISTSIZE`(default 1000)
- Can check the size by running `echo $HISTFILESIZE` or `echo $HISTSIZE`
- `bash_history` is only updated when terminal is logged out
- To run command from `history`, do `!<history-order-number>` (E.g. `!17` will run the number 17 command in `history`)
  - `!!`: rerun last command
  - `!-n` where n is the last nth command
  - `Ctrl + R`: search for bash history command
  - `Ctrl + G`: exit history search (Ctrl+R)
  - `Ctrl + D` <history number>: remove a history entry by its number
  - `history -c`: remove the whole history

#### Other variables
- `HISTCONTROL`: control how you want to manipulate whether or not your command will be added to your history
  - `ignoredups`: ignore duplicates command from being added to history (make history cleaner)
  - `ignorespace`: ignore the command if it starts with a white space (e.g. ` who` will not be added to history because it has white space in front)
  - `ignoreboth`: combination of `ignoredups` and `ignorespace`
  - To change a value of variable do `HISTCONTROL=<value>`, but it will only apply for that session. To make it permanent, need to put that new value to .bashrc file (echo “<variable>=<value>” >> .bashrc)
- `HISTTIMEFORMAT`: adjust time format in your history
  - Make time format available in history: `echo "HISTTIMEFORMAT=\"%d/%m/%y %T\"" >> ~/.bashrc`

### Root Access

#### Root user
- Root user: super user or administrator
- Only one root user per one Linux system
- Not recommended to use root user for ordinary task, only for administrative tasks or task that need root permission.

#### Accessing root
- `sudo su` : starts a new shell with root user power
- `sudo su -`: same with `sudo su` but will start from new shell (as in without the -, it will start at the same directory of the non-sudo user)
After you sudo, you gain root privilege for 5 MINUTES
- `sudo -v`: to refresh your sudo power to 5 minutes again (while its still active)
- `sudo -k`: to kill your sudo power immediately, and will require password the next time you want to sudo
- `sudo passwd root` (setting password for root) [my password: makantai)
- `passwd username` (changing a user’s password)

#### Terminology
- root directory (`/`) , refers to the user’s main directory
- "Slash-root directory" (`/root`), sub directory of root directory for mainly administrative purposes


## Linux File System

### Intro

- File System: controls how data is stored and retrieved; manage files, their rules and their name. 
- File: group of data. In other words, File System is a logical collection of files in a partition or a disk
- In Linux: everything is a File (including physical device like USB stick, and directory is a special kind of file). 
- If something isn’t file in Linux, it is a process

##### Visual Representation
![File system visual representation](static/images/linux_file_system.png "File system visual representation")


- For a physical device, like a USB stick, it needs to be mounted under an existing directory under root 
instead of partition in other OS such as windows. To check the mounted device: `df -h`

![partition](static/images/df_h.png "partition")

#### Filesystem Hierarchy Standard (FHS)

##### Standard directory standard in linux distribution
- `/bin`: containing binaries or executable files that are available to all users (e.g. `ls` command)
- `/sbin`: executables only availble to su user(e.g. `fdisk` to monitor disk partition)
- `/dev`: device file (directory), contains device files such as hard disk or partitions. 
  - E.g. `ls -l /dev/sda1` : first logical partition
- `/etc`: etcetera, dumping ground for system files. In modern linux distributions, it contains most configurations files
- `/lib`: shared library files used by different applications, not to touched directly
- `/media`: where external storage automatically mounted when plugged and tried to accessed (usb stick or flash disk)
- `/mnt`: mount point if you don’t know where, not used a lot 
- `/tmp`: contains temporary files, saved by applications that are running. Can be deleted anytime without warning
- `/proc`: info about computer hardwares such as memory and CPU, kernel, etc
  - e.g `cat proc/cpuinfo` to see information about the CPU
- `/sys`: contains about devices, drivers, and kernel system 
- `/srv` : data for server
- `/run`: temp file system runs in RAM, context vanishes when system reboot. Only used by processes
- `/home`: main user directory (can be accessed by `~`)
- `/usr`: used to be main directory, but replaced by /home today. Now stored many subdirectories for commands
- `/var`: variable length files such as logs

#### Absolute and Relative paths

- Main commands: `cd`, `pwd`, `ls`
- Absolute: starts with slash(/)
- Users home directory (~)
- Current directory(.), parent directory(..)
- `tree` command
  - `sudo apt tree`
  - `tree /etc`
  - `tree /etc -d .` / `tree /etc -f .`

### Viewing Files

#### `cat`
- Usage: `cat <file-path>`
- Arguments: can be multiple file to display (e.g. `cat main.go .gitignore`)
- Options: 
  - `-n` to display the line number (e.g. cat -n main.go)
- Can be use to concat files (e.g. `cat file1 file2 > file_combine`)

#### `less`
- `more <file-path>`: show file(s) separated by line
- `less <file-path>`: show file(s) as pages, can switch between pages 
  - e.g. `less file1 file2`
  - navigation guide in COMMAND section of `man less`


#### `head`, `tail`, `watch`

- `tail <file-path>`: show the last 10 lines
- Option:
  - `-n` : shows the last n lines. If `+` is appended before the n (e.g. `tail -n +20 file`), it will shows all the line after line 20
  - `-f`: real time viewing file changes
- `head <file-path>`: shows the first 10 lines, opposite of tail
- `watch`: execute command output repeatedly 
  - For examples, can be useful with linux’s `ifconfig` to see the packets change and received files (e.g `watch ifconfig`)
  - Options: 
    - `-n`: interval time before updating
    - `-d`: highlight differences between update

### More File Operations

#### Creating Files & Directories

##### `touch`
- `touch <file-name>`: create file(s)
  - if file already exist will change the `atime`, `ctime`, `mtime`
- `mkdir <dir-name>`: create directory(s)
  - Common Options: 
    - `-v` (verbose)
    - `-p`: will create intermediate directories if existed (e.g. mkdir first/second/third). This assuming first and second doesn’t exist

#### Copying files and directories

##### `cp`
- `cp <origin> <destination>`: copy file
  - If destination is not exist, cp will create it
  - If destination already exist, cp will overwrite it
  - If origin more than one, destination must be directory (cp file1 file2 file3 dest-dir)
  - If origin and destination are both directories, it will copy the origin files to the directory and creating necessary directory inside destination if needed (cp dir1/* dir2). MUST use `-r` option
  - Options
    - `-v`: verbose
    - `-i`: prompting (“overwrite to <dest-file>?”
    - `-r`: recursive, used to copying directory to another directory 
    - `-p`: preserving file attributes after copying

#### Moving and renaming files and directories

##### `mv`
- `mv <origin path> <destination path>`: moving multiple files, destination must be a directory 
  - If destination doesn’t exist, it will simply rename (and/or move) the origin file to the destination file 
  - Options
    - `-i`: prompting
    - `-u`: move only when newer file is being overwritten or if file didn’t exist yet

#### Removing files and directories

##### `rm`
- `rm <file>`: remove file(s)
- Option: 
  - `-i`: prompting
  - `-v`: verbose
  - `-r`: recursive remove
  - `-f`: force remove

- No recovering from removed files, best practices are to:
  - Always  use tab for autocomplete
  - When using pattern matching like * or wildcards can `echo` the matching files first before removing them

##### `shred`
- `shred`: overwriting the files couple of times before removing it
  - To prevent other people of recovering data, because the file is removed but the pointer to that file might still exist
  - Options:
    - `-n`: number of iterations before removal (default is 3)
    - `-u`: remove files after overwriting 
    - `-v`: verbose
    - `-f`: force permission to allow writing

### Finding Files and String

#### Locate
- `locate <keywords>`: locate files with matching keywords
  - But it stores the the files in its own database instead of the file system directly
  - By default the keyword matching pattern is ```*<keyword>*```
  - To find exact keyword can do : `locate ‘\<keyword>’`
  - Options
    - `-b`: basename, will only search the basename file (e.g. /etc/password is located but /password/test.txt is not)
    - `-e`: check if the file exists, because location use its own database instead of file system directly
    - `-i`: make it so case insensitive (default case sensitive)
  - Keyword can also use regex to pattern match
  - `sudo updatedb` to update the locate db 

#### Which
- `which <binary>`: will return the path of an executable file
  - Example:
    - `which rm` , `which ls`, etc
    - Combine: `which rm ls grep`
  - Options:
    - `-a`: find more than one executables with the same name

#### Find
- `find <location> criteria`
- Complex but will find in file system instead of its own database like locate
- Options
  - `-name`: find matching file name, can use pattern matching too (e.g. `find . -name “*.sql”` , `find . -name “*rep*”` )
  - `-size`: criteria by file size, can prefix by “+” or “-” to signify greater of less than specified file size(e.g. `find  . -size +100M` find more than 100 Mb, `find . -size -10k` find less than 10 kb)
  - `-type`: type of file 
  - `-mtime`, `ctime`, `atime`: by modified, changed, access time (integer in term of days). Examples:
    - `-mtime -7` : find files that is modified less than 7 days ago
    - `-mtime +7`: find files that is modified more than 7 days ago
- Find and Exec
  - Combine find output and exec for each found files
  - Example:
    - `sudo find /etc/ -mtime -7 -type f -exec cp {} /root/backup \;`
      - The `{}` acts as placeholder to each of the file 
      - The `\;` at the end is simply the construction structure (must be there for each exec )
      - Alternative : `-ok` is interactive(prompt) version of `-exec`

#### Pattern Searching (`grep`, `strings`)

##### `grep`
- `grep`: Searching for string patterns by matching keywords inside file
- Options
  - `-w`: find exact whole word (by default ```*<keyword>*```)
  - `-i`: case insensitive
  - `-n`: shows the line number which the keyword is found
  - `-a`: treat non-ascii files (like binary files) as ASCII file
  - `-R`: recursive, if target is a directory 
  - `-s`: suppress error
  - `-c`: just shows the number of matches (like grep … | wc -l)
  - `-A`: shows n lines after match(e.g. -A 3 will shows 3 lines after match)
  - `-B`: same with `-A` but before match
  - `-C`: combination of `-A` and `-B`
  - `-v`: inverse the match
- Examples
  - `grep -i “root” filename`
  - `ifconfig | grep ether`

##### `strings`
- `strings`: Searching for strings in binary files (strings)
- Find printable strings in a file, binary, objects, etc
- Examples:
  - `sudo strings /dev/mem`

##### `cmp`, `diff`, `sha256`
- Comparing Files (cmp, diff, sha256)
- `cmp <file1> <file2>`: compares files byte by byte and shows on which line the files differs, but not tell you how they differ
- `sha256sum <file1> <file2>`: also compared the hashed content of a file, if a single bit is different hashed output of the file would be different 
- `diff <file1> <file2>`: display the differences between file line by line 
  - `<` symbolise the first file argument
  - `>` symbolise the second file argument
  - Options: 
    - `-B`: ignore blank lines
    - `-i`: ignore case differences
    - `-w`: ignore whitespace
    - `-c`: more complex comparison

### VIM


### Hard and Symlinks

#### The Inode Structure

![inode](static/images/inode.png "inode")
- Each file on the disk has a data structure called index node or inode associated with it.
- This structure stores metadata information about the file such as the type, file's permission, file's owner and group owner, timestamp information, file size and so on.
- It actually contains all file information except the file contents and the name.
- To see inode number (is unique) do `ls -i` 
- Can copy the inode by using `ln` command (e.g. ln a.txt b.txt)
- What this will do is making b.txt point to the same inode structure as a.txt (a.txt and b.txt point to the same memory)
- When using `ls -l` , you can see the number of hard links of a.txt is increment by 1

## User Account Management

## Linux File Permission

## Linux Process Management

### Processes and Linux Security Model

#### Process
- Process: running instance of a program. Has its own memory space
- New process created when you execute command (only executable file command, not shell built in, you can check this by doing `type <command>`)
- Process properties:
  - PID - a unique positive number
  - User
  - Group
  - Priority/Nice
- Type of processes:
  - Parent: process that has created >= 1 child processes
    - For example, `bash` is a parent process of command `who` 
  - Child 
  - Daemon: processes which run in the background, not interactive, in linux daemon names ends in “d” (e.g httpd, sshd, named, mysqld, systemd)
  - Zombie (defunct): Terminated process whose data has not yet been collected. Usually removed quickly and not using system resources
  - Orphan: Parent process terminates before child processes
    - E.g. Closed terminal while processes still running

#### Thread
- Thread: multiple can exists inside a process, share same resource on the same process
- Task: synonyms with process

### Listing Processes

#### `ps`
- `ps`: displays a header line, followed by lines containing information about all of your processes that have controlling terminals
  - Output:
    - PID: process id
    - TTY: name of controlling terminal
    - TIME: cumulative CPU time of the process
    - CMD: name of the command to start the process
  - Options
    - `-c`: list all processes
    - `-e`: displaying all processes that are running
    - `-f`: displaying detail information about the processes (usually combined with `e`)
    - `-aux` (can also `ps aux`): display additional output information, about resource used by the process
    - `-u`: process of specific user `u`
  - Additional Output (using `-f`)
    - PPID: parent process id
    - STIME: process starting time
    - TTY: If tty is `?`, doesn’t attached to terminal (either system service or daemon)
    - Additional Output (using `aux`)
    - %CPU: CPU utilization of the process in %
    - %MEM: how much memory used in %
    - VSZ: virtual memory size
    - RSS: size of physical memory that process is using
    - STAT: process state using code
      - `s`: sleeping
      - `r`: running
      - `z`: zombie
      - `t`: stop
      - `i`: idle
      - Suffix `<`: high priority
      - Suffix `N`: low priority
      - `R+`: running process
    - Sort example 
      - Ascending: `ps aux –sort=%mem | less` 
      - Descending(add `-`) `ps aux –sort=-%mem | less` 
    - Common usage with grep
      - `ps -ef | grep sshd`: get all processes, and check if `sshd` is running
      - ** Be mindful, e.g.: `ps -ef | grep not_exist_process` will return a process. It selected itself (the grep command)


#### `pgrep`
- `pgrep`: Get pid of a process directly 
  - Example: `pgrep sshd`
- Option:
  - `-l`: will show both pid and process name

#### `pstree`
- `pstree`: display hierarchical tree structure of the process
- Option`:
  - `-c`: unmerge identical branches

### Signals and Killing Processes

#### Signal
- Default signal : SIGTERM(15)
- `kill -l`: gets a list of signals
- Can use the signal number to send that signal to a process 
  - Examples: `kill -2 1234`
- To get name of process can use `pgrep` or `pidof` command
- System related files processes like `sshd` are stored in `/var/run` (e.g. `cat /var/run/sshd.pid` will return pid of sshd process)
- SIGTERM (soft kill, can be ignored by a process) vs SIGKILL (hard kill, cannot be caught by a process )

#### `killall`
- `killall`: Kills all processes with a particular name
- Name must match, cannot be partial
- Example: `killall sleep`

#### `pkill`
- `pkill`: similar to`killall` but can accept partial skill
  - Example: `pkill slee` will kill all sleep processes

### Foreground vs Bakground Processes

#### Foreground process
- Started by the user, once a foreground process started in a terminal, it can’t run another process
  - Example:
    - `sleep 15` starts a foreground process for 15 seconds
  
#### Background process: 
- Non-interactive , started by the system. Usually executed by system services or by the users. Can start another process within the same terminal
  - Example: 
    - `sleep 20 &` :ends with & meaning background process

### Job Control

#### Job
- A job is basically a background process. On top of process id, a background process is also given a job id 
  - Example:
    -  `sleep 20 &` => `[1] 15766`: where 1 is the job id and 15766 is the process id
 - `jobs`:
   - This will display all jobs in the current terminal session.
   - Processes instantiated by terminal only exist while that session is alive
   - If a terminal is closed while the process by that terminal is running then an `HUP` (Hang Up) signal is called and it will kill all that session processes.
     - To avoid `HUP` signal, can use `nohup` (e.g. nohup <any command>)
     - Output will be directed by default to `nohup.out`

## Networking in Linux

### Getting information about the Network Interfaces (ip, ifconfig)
<i>I will be focusing on the `ip` command</i>

#### `ifconfig`
- install: `sudo apt install net-tools`
- `ifconfig -a`: Display all network interfaces and IP address

#### `ip`
- `ip address show`/ `ip a`: display all network interfaces and IP address
  - Where `address` is the object
  - `show` is the command/verb you do the the object
- Example output of `ip a`
- `lo` and `eth0@if2476` describes the network interface name
- `UP` means that the network is active
- Other infos: ip address(`inet`), broadcast address, mac address(`ether`), network mask, etc

- Network interface name
  - Prefix `en`: ethernet
  - Prefix `wl`: wireless lan
  - Middle `p<number>`: port number
  - Middle `s<number>`: slot number
  - `ip address show dev <interface-name>`/ `ip a s dev <interface-name>`: display ip info of a specific network interface
  - `ip route show`: show IP routing table


### Configuring the Network on the fly (ifconfig, ip, route)
`ip link set <interface-name> <status>`: set a status(“up”, “down”) of an interface
Change IP address:
Delete current: `ip address del 192.168.0.111/24 dev enp0s3`
Add new: `ip address add 192.168.0.222/24 dev enp0s3`
Change default gateway:
Delete current: `ip route del default`
Add new: `ip add route default via 192.168.0.1`
### Setting up static IP on ubuntu (netplan)

### Testing and troubleshooting network connectivity
- `ping`: send echo request packet to a domain
  - Example 1: send to `ubuntu.com`. Resolved to different domain, hint: each IP can host multiple domains, the DNS reverse lookup and find different domain under the same IP
  - Example 2: to not do DNS reverse lookup can use `-n`
  - Example 3: Unreachable host
  - Example 4: send multiple packets
    - `icmp_seq`: sequence order, ideally want them to be in order
    - `ttl`: time to leave, number of routers between sender and receiver
    - `time`: RTT (sender -> receiver -> sender)
      - <30 : excellent
      - 30-50: average
      - 50-100: somewhat slow
      - `>` 100: very slow
  - Options
    - `-i`: delay between packets (delay 1, faster delay require sudo access )
    - `-q`: summary
    - `-t`: ttl number
      - For example: `-t 1` meaning the first router from source to destination

- How to troubleshoot:
  - Get the default gateway(`ip route`, default)
  - Ping the gateway (if fail, have problem in your LAN)
  - Ping any public  DNS in the internet (e..g 8.8.8.8 Google DNS), (if fail, internet connectivity issue)
  - Ping any domain (e.g. google.com), if fail then DNS issue

### Using SSH
- Network protocol used by system admin to securely configure a remote system over unsecured network connection
- Troubleshooting SSH
- Securing the OpenSSH Server (sshd)
- Copying files over the network (scp)
- Synchronizing files and directories using rsync
- Using rsync over the network 
- Using `wget`
- Checking for listening ports (netstat, ss, lsof, telnet, nmap)

## Software Management

## System Administration

## Bash Scripting

# Extras

## Use AI and Natural Language to Administer Linux Systems (ChatGPT & ShellGPT)

## Configure a Linux Server from Scratch (VPS,DNS,WEB,PHP,MySQL,Wordpress)

## Security: Information Gathering and Sniffing Traffic

## IPFS: Interplanetary File System

## Netfilter and Iptables Firewall

## Security: SSH Public Key Authentication

# Challenges

## The Linux Terminal

## Linux File System

## File Permissions

## Process Management

## Networking on Linux

## Software Management 

## System Administration

## Bash Scripting

## Netfilter and Iptables

# Others


