---
date: 02-09-21
id: 28
path: Courses
tags: []
title: Linux LPIC-1 and CompTIA notes
type: note
---

Course URL: https://www.cbtnuggets.com/it-training/lpi-linux-lpic-1-and-comptia-linuxplus-prep

Integrated Hardware Devices

Hardware devices are present files in the /dev directory. Examples:

    eth0, eth1, eth2...
    sda, sdb, sdc.... (storage device)
    hda, hdb.... (IDE hard drives)
    video0.....

Drive partitions:

    /dev/sda1
    /dev/sda2

Drivers (modules):

    lsmod (list currently-active modules)
    modprobe (starts modules)

Removable devices

Hard drives (SATA, IDE, eSATA), USB drives, NAS.

Hot plug or cold plug.

Tools:

    lsusb (lists USB devices)
    usb-devices (detailed properties of USB devices)
    lspci (lists PCI devices)
    dmesg (lists system events/messages printed by kernel)

Dealing with devices

procfs - originally, process information was put into the virtual filesystem, /proc/. This worked so well, that it was used for everything and got messy.

sysfs - in kernel 2.5, sysfs was introduced specifically to contain system info (devices, etc.)

udev - in kernel 2.6, udev was introduced, which populates the /dev/ directory.

dbus - created as a signalling system to allow software to interact with hardware devices (e.g. allows OS to generate a desktop link to a USB drive when inserted).
The Linux boot process

Boot process:

    POST (BIOS/UEFI)
    MBR - master boot record. 512b of first HD sector. Contains bootloader (GRUB) and partition information
    GRUB - or now GRUB2, launches kernel or initrd
    Initrd - Initial Ramdisk, minimal Linux kernel which detects what kernel modules to load to run on hardware, mounts hard drives, then hands off to the main kernel
    Kernel - starts init process. Init is the process that starts all the other processes on a running system.

Boot time kernel commands and the Init system

Single user mode - must be at console. Can (sometimes) change root password.

Need to interrupt the GRUB bootloader (normally just be pressing any key), then loading a kernel choosing to pass additional arguments (varies between systems).

Different init systems exist:

    systemvinit - runlevels, numbered order, lots of symlinks, not very efficient.
    upstart - faster startup, start order not just numerical, backwards compatible. Developed by Ubuntu, now being deprecated in favour of systemd.
    systemd - as upstart, not as backwards-compatible. Uses binary code, not just scripts.

Managing runlevels with sysvinit

Runlevels - define the mode of the system. Debian/Ubuntu:

    0: Halt/power off
    1: Single user mode (rescue)
    2: full, multiuser, GUI
    3-4: nothing
    6: Reboot

Display runleve: runlevel

Changing runlevel: telinit X

Setting default runlevel: /etc/inittab
Managing boot targets with systemd

Linux systems can have several modes of operation:

    systemctl get-default
    systemctl isolate graphical.target
    systemctl set-default multiuser.target

The telinit commands can also be used.
Halting and rebooting your system

Runlevels & boot targets:

telinit 0
systemctl isolate poweroff.target
telinit 6
systemctl isolate reboot.target

Tools:

    poweroff - sends ACPI power off signal
    reboot
    wall - sends a message to all terminals
    echo "Message" | wall
    shutdown -r now (reboots immediately)
    shutdown -h +2 (shuts down to halted state in 2 minutes)
    shutdown -P 13:26 (powers off at time)
    shutdown -P +3 "Alas, we are powering down in 3 minutes!"

Partitions and mount points

Hard drives can be partitioned into up to 4 logical partitions (each of which can be partitioned into multiple extended partitions).

Swap space can also exist on the drive as a partition.
Bootloaders: GRUB & GRUB2

GRand Unified Bootloader system

GRUB config: /boot/grub/menu.lst (manually edited)

GRUB2 config: /boot/grub/grub.cfg (auto-generated, not edited manually)

    Edit /etc/default/grub (sets options)
    grub-mkconfig -o /boot/grub/grub.cfg (generates grub.cfg)

Using apt

apt-get update | upgrade | dist-upgrade | install | remove | purge
apt-cache search | show
aptitude search | show | install | safe-upgrade

Repeat setup/package installation:

dpkg-reconfigure PACKAGE

Environment variables

List all env variables: env

Use export VARIABLE to set a variable (can append to existing) and make it available to child processes.

Use unset VARIABLE to remove a variable.

Example: PATH variable. Add to an existing PATH variable:

export PATH=/my/new/path:$PATH

Manipulate text

cat, head, tail, split:

head filename  # displays the first n lines of filename
tail filename  # displays the last n lines of filename. Often used to monitor a logfile (can follow a file as it is appended to).
cat file1 file2 file3  # displays the listed files, concatenated together
split -l 5 filename prefix  # splits filename each 5 lines and puts the output into files prefixed by prefix. Can also split on bytes using -b (e.g. split -b 10 filename) .
paste file1 file2  # joins the same line in each of the two files (line 1 to line 1, etc.), outputs new file
join file1 file1  # joins lines in each file based upon a preceding identifier (e.g. an integer value)
expand
unexpand

Manipulating text with sed

sed = "stream editor"

sed 's/this/that/g' filename - this substitutes the first pattern for the second pattern, globally (not just once on each line).

Patterns can be a string or a regular expression. Output of sed can also be used as input to sed, for multi-pass substitution.
The find command

Find by name, size, date:

find <location> <options>
find /home -type d|f  # directory or file
find /home -name <NAME>	 # file name
find /home -iname <NAME>  # case insensitive name
find /home -mtime 10  # modified within last 10 days
find /home -mtime +|-X	# modified more/less than X days ago
find /home -cmin -60  # changed in last 60 minutes

Do something with the results:

find <location> options -exec <bash command;>

Using tar, cpio and dd (archiving)

Block device: hard drive is divided into a number of blocks.

File system: abstract organisation of file references.

dd: only works with block devices. Used to read input from a block device and write output to another block device.

dd if=/dev/sda1 of=/dev/sdb  # Copy one block device to another.
dd if=/dev/sda1 of=/tmp/backup.iso

tar: originally used for tape archives.

tar [options] [output] [target to be archived]
tar -cvf file.tar /home/ashleyf/projects
tar -xvf file.tar

cpio: takes a list of files/dirs and makes an archive.

ls | cpio -o > ls_archive.cpio  # takes the output of ls, redirects that to a file
find . | cpio -ov > find_archive.cpio
cpio -id < ls_archive.cpio  # takes a file as input, creates a list of files and directories

Compressing files with gzip & bzip2

gzip <target>  # Will create a <target>.gz file and delete the target
gzip bigfile.doc
gunzip bigfile.doc.gz
bzip2 bigfile.doc
bunzip2 bigfile.doc.bz2

tar with gzip & bzip2:

tar -zcvf bigfile.tar.gz  bigfile.doc  # create tarball with gzip
tar -jcvf bigfile.tar.bz2 bigfile.doc  # create tarball with bzip2
tar -zxvf bigfile.tar.gz
tar -jxvf bigfile.tar.bz2

Standard Input (STDIN), Output (STDOUT) and Error (STDERR)

Defaults:

    STDIN: keyboard input
    STDOUT: console
    STDERR: console

Redirect to/from files:

<  # redirects STDIN
>  # redirects STDOUT
2> # redirects STDERR
ls > outfile  # redirect stdout to outfile
ls nonexistent > outfile 2> errfile  # redirects both stdout and stderr
read MYVAR < file.txt  # redirect stdin from a file

Pipes, Data Redirection and Xargs

Piping STDOUT of one app to STDIN of another app.

# Basic redirection using pipes
cat file.txt | grep foo
ls | grep .mp3

Not every program can accept STDOUT in its STDIN (e.g. rm). For this we use xargs. We can pipe STDIN to xargs and it will run the specified command and append the input.

ls | xargs rm -rf  # Will delete everything returned by ls

tee: program that takes STDIN, prints it to the screen and redirects it to a file:

echo "Some output" | tee file.txt

Redirect STDERR back to STDOUT:

ls missing_dir > results.txt 2>&1   # Redirects stdout to a file, redirects stderr to stdout

Foreground & background jobs

fg, bg, jobs

# Run a job in the background via ampersand
sleep 9000 &
jobs  # List running jobs

Suspend a job via ctrl-z. Terminate a job via crtl-c. Use fg and bg to put jobs into the foreground and background:

jobs  # List running jobs
bg %1  # Send current suspended job to background
fg %1  # Bring nominated job to the foreground

Prevent a job from being closed with a terminal via nohup and disown:

nohup sleep 9000 &  # Prevents a job being closed with the terminal
disown %1  # Prevent an existing running job being closed with the terminal

The screen program can be used to create a persistent terminal session. The current screen session can be "detached" using crtl-a, d. The screen session can then be reattached to a new terminal window:

screen   # Start a screen session
# Detach using ctrl-a, then pressing d
screen -dr   # Start screen, detaching it from any existing terminals and re-attaching it to the current terminal

PIDs, Signals and the kill command

Every process has an id (PID). Signals include the following:

    SIGHUP (1) - Hangup
    SIGINT (2) - Interrupt (sent via ctrl-c)
    SIGKILL (9) - Kill
    SIGTERM (15) - Terminate

sleep 1000 &  # Start a long running process in the background
ps aux | grep sleep  # Lists any processes containing the work 'sleep'
kill -15 <PID>
kill -9 <PID>

Advanced process management

Using killall:

sleep 9999
sleep 9999
sleep 9999
killall sleep  # Needs to be the entire process name
killall -9 sleep

Using pkill & pgrep:

pkill sleep
pkill slee  # Doesn't need the full name
pgrep slee
grep -a slee  # Includes process name
grep -af sleep  # Searches the full execution string

Process priorities and nice levels

Each process has a nice level. These define how a CPU prioritises to undertake each process. Default nice level is 0, higher priority goes to -20, lower priority goes to 20.

-20			-10			0		10				20
Highest				Default					Lowest

Starting an application with a nice level (note that the root user is the only one that can start a process with a priority higher than default):

nice <app>  # Default level
nice -10 <app>  # Sets nice level as 10 (lower priority)
nice --15 <app>  # Sets nice level as -15 (higher priority)

Regular expression with grep, egrep and fgrep

grep: searches for text, returns lines when found. Handles basic regex.

grep <term> <location>
grep foo file.txt
cat file.txt | grep foo  # Feed STDIN to grep
grep ^foo file.txt  # Regex: start of line
grep "search term" --include "*.txt" .  # Search all .txt files at current location recursively.

Regex syntax:

    ^ start of line
    $ end of line
    . any non-blank character
    (X|x) or (inside a group)

egrep: works with regular expressions

egrep '^(P\P)....$' file.txt  # Find lines starting with P or p, followed by 4 four characters and then the end of line.

fgrep: fast grep, doesn't evaluate regular expressions at all.

fgrep $$ file.txt  # Searches for literal "$$" inside file.txt

GPT & MBR

MBR: Master Boot Record, introduced 1983, limitations in partition size (2 gb) and number (4 primary partitions, with extended partitions able to be presented to the OS as more than one).

GPT: GUID Partition Table, new schema, backwards compatible with MBR, no limitations.

BIOS: Basic Input Output Syste, only supports MBR.

UEFI: Unified Extensible Firmware Interface, native GPT support.

Using MBR:

sudo fdisk /dev/sdX

Using GPT:

sudo gdisk /dev/sdX

Formatting partitions in Linux

ext2/3/4 - Extended File System. ext3/4 have journaling.

RieserFS - very space efficient with small files, not used much due to licensing strangeness.

BTRFS - snapshots, compression/deduplication features.

XFS - good for large partitions, getting outmoded.

Swap - pagefile (virtual memory), can use a file in Linux but usually an entire partition.

mkfs.ext4 /dev/sdb1
mkfs -t ext4 /dev/sdb1  # Synonymous with the previous command
mkswap
parted  # Partition Editor

Maintaining filesystems

fsck - Filesystem check. Filesystem must not be mounted when running fsck on them. Frontend to fsck.ext2, fsck.ext3, etc. Auto-detects filesystem, is smart about XFS.

tune2fs, dumpe2fs, debugfs - rarely used.

XFS tools:

    xfsprogs package must be installed.
    xfs_info: shows information.
    xfs_check: complete check of filesystem (no longer rec. to use, use xfs_repair -n instead).
    xfs_repair: checks and fixes errors.
    xfs_repair -n: checks only, no fixes.

 

fsck /dev/sdb1  # Will only work if the filesystem is not mounted, check only, no fixes
fsck -f /dev/sdba  # Will fix problems (interactive confirmation)

Mounting and unmounting filesystems

mount/umount:

#mount -t <optional type> -o <options> <device> <mount point>
mount -t ext4 /dev/sdb1 /home
umount /home

fstab - makes permanent mount points:

 # <device> <mount point> <type> <options> <dump> <pass>
/dev/sdb1 /home auto defaults 0 0
/dev/sdb2 /mnt/backup ext4 rw,user,noauto,exec,utf8 0 0

Quotas

Drive quotas can be hard or soft. Installation:

apt-get install quota

In fstab, additional options are required: usrquota, grpquota. E.g.:

/dev/sdb1 	/mnt   auto		defaults,usrquota,grpquota	 0	 2

Then:

quotaon /mnt  # Can also use quotaon /dev/sdb1
# Create a report of quotas:
quotacheck -cug /mnt
# Run a report on usage:
repquota /mnt
# Configure quota for a user:
edquota -u username /mnt

File ownership and permissions

File and directory permissions:

-rw-r----- owner group filename
drwxr-x--- owner group directoryname

First character: file (-) or directory (d).

Next 3 chars: read/write/execute for owner.

Next 3 chars: read/write/execute for group.

Last 3 chars: read/write/execute for everyone else (other).
Changing permissions and creation masks

Use chmod, chown

chmod ugo+w filename   # Add write permission for user, group and others.
chmod o-rwx filename   # Remove read, write and execute from others.
chmod a+x filename   # Add execute for all.

Octal notation:

-| u | g | o
-|rwx|rwx|rwx
 |421|421|421
 
chmod 755 filename   # Added all for user, read & execute for group and others.
chmod 750 filename

The creation mask (umask) is the default ownership pattern for newly-created files, and is set system-wide. The umask takes the system default permissions (files = 666, directories = 777) and subtracts some permissions (e.g. file umask = 022 → 666-022 = 644, directory umask = 002 → 777-002 = 775).

Umask defaults (normally): /etc/profile, /etc/bashrc and/or /etc/login.defs
SUID, GUID and the sticky bit

SUID (Set User ID)

    File: executes with the permissions of file owner (e.g. /bin/ping executes with the permissions of root user). THIS IS POTENTIALLY A HUGE SECURITY RISK!
    Directory: no effect

GUID (Group user ID

    File: executes with permissions of group.
    Directory: new files have group membership of the directory.

Sticky bit:

    File: no effect.
    Directory: only the file owner can delete the file (e.g. files in the /tmp directory).

Setting these:

u+s  # Sets the SUID
g+s  # Sets the GUID
o+t  # Sets the sticky bit

Settings via octal notation: SUID = 4, GUI = 2, sticky bit = 1. E.g.:

chmod 0755 filename  # The first digit sets special permissions. It defaults to "0" at all times.
chmod 4755 filename  # Sets the SUID on the file.
chmod 1777 directoryname  # Sets the sticky bit on the directory.

Hard and soft (symbolic) links

Hard linking links to an inode on a drive (can't be linked across drives). Changing the linked file name will not affect the hard link.

Soft (sym-) linking links to a file name (not inode). Can be linked across drives. Changing the linked file name will break a symlink.

Can list inodes using: ls -li

# Create a hard link:
ln /path/filename /path/hardlinkname
# Create a symlink:
ln -s /path/filename /path/symlinkname

File Hierarchy Standard (FHS)

The Linux File Hierarchy Standard (FHS) was developed to provide some consistency to where files are stored:

/			# Root directory
/bin		# System binaries
/boot		# Boot files & boot loader
/dev		# Devices (sda, etc.)
/etc		# Configuration files
/home		# User files
/lib		# System libraries
/media		# Removable storage
/mnt		# Temporary file system
/proc		# Virtual file system, current state of system processes
/root		# root user home dir
/sbin		# System binaries
/tmp		# Temporary storage area
/var		# Variable files: logs, etc.
 
# Also:
/usr				# User files (often read-only)
/usr/bin			# User binaries
/usr/lib			# Libraries for user binaries
/usr/local			# Host-specific files
/usr/local/bin		# Host-specific binaries

Find and locate

# which, whereis
# which <binary>
which ping

find: search files in real time:

find <location> -name filename
find / -name filename
find . -type f -name filename

locate: uses a database to find files (fast), but database is only updated periodically (via updatedb)

locate filename
updatedb  # Updates the database of files

User profiles and system profile

Login (SSH) shell profile start process (executes these in order):

    /etc/profile
    /etc/profile.d/*
    /home/user/.bash_profile | .bash_login | .profile (first one found only)
    /home/user/.bashrc

Interactive (non-login) shell start process:

    /etc/bash.bashrc
    /home/user/.bashrc

User profile skeleton: /etc/skel
Scripting basics

Command interpreter (shebang), first line of an executable script defines which interpreter to use:

#!/bin/bash

# This is a comment.
echo "I can talk!"
 
# Define a variable, then use it:
NAME="Ashley"
echo $NAME
 
# Command substitution, enclose a command in $(....):
TESTCMD=$(ls -la)
echo $TESTCMD

To execute a script, either make it executable or use e.g. bash: bash myscript
Scripting conditional and loops

if [condition]
then
	do this stuff
else
	do this other stuff
fi

Example:

#!/bin/bash
if ["YES" == "YES"]
then
	echo "They match!"
else
	echo "They don't match"
fi

Read input:

#!/bin/bash
echo "Do you like this?"
read ANSWER
 
if [$ANSWER == "yes']
then
	echo "Awesome!"
else
	echo "Laaaame"
fi

For loop syntax:

for VAR in [some sequence]
do
	stuff with $VAR
done

Example:

#!/bin/bash
 
for VAR in 1 2 3 4 5;
do
	echo $VAR
done
 
for VAR in {1..10};
do
	echo $VAR
done
 
for VAR in $(ls);
do
	echo $VAR
	cat $VAR
done

While loop syntax:

while [condition is true]
do
	stuff
done

Example:

#!/bin/bash
 
VAR=1
while [$VAR -lt 10]
do
	echo "VAR is $VAR"
	let VAR=VAR+1
done
echo "done"

SQL data management

sqlite3
X11 configuration

 
Display managers

 
Managing Users & Groups

useradd (options) <username>

    -d (/home/dir)
    -m (create home dir)
    -s (shell)
    -G (additional groups)
    -c (comment, e.g. full name)

usermod

    As above
    -L (lock account)
    -U (unlock account)
    -aG <group> (add to group)

userdel

    -r (remove home dir)

groupadd (options) <groupname>

    -g N (group number, e.g. groupadd -g 10000 marketing)

groupmod, groupdel

passwd <username>
Password, Group and Shadow databases

/etc/passwd fields:

username:password:user id:guid:name,:home dir:shell executable

/etc/shadow: only readable by root or shadow group. Contains encrypted passwords and details about them.

sudo chage -l <username> (lists details about user password)

/etc/group

groupname:password:group id:members

/etc/gshow - contains information about group passwords (mostly empty).
Cron

* * * * * command_to_execute
| | | | |_ day of week 0-6 (Sunday=0)
| | | |___ month 1-12
| | |_____ day of month 1-31
| |_______ hour 0-23
|_________ minute 0-59
 
# Examples
@reboot foo			# On reboot
@daily foo			# Daily
2 13 * * * foo  	# 13:02 daily
5 * * 6 0 foo		# 5 minutes past every hour, each Sunday in June
1,6,12 * * * * foo	# 1, 6 and 12 minutes past every hour
*/5 * * * 1-5 foo	# Every 5 minutes, Monday - Friday

At scheduler (once-off jobs):

    at, atq, aq:
    at now + 5 minutes

Security:

    /etc/cron.allow (whitelist of users)
    /etc/cron.deny (blacklist of users)
    /etc/at.allow
    /etc/at.deny

 
System job scheduling (cron and anacron)

System crontab, cron.d:

    /etc/crontab
    /etc/cron.d (directory of crontab files)

/etc/cron.hourly, cron.daily, etc.:

    directories of executable script files that are run at the schedule as root

Anacron - if a cron.daily, weekly or monthly was missed, it notifies at boot time.

    Time stamps in /var/spool/anacron

Localisation & internationalisation

/etc/timezone

/etc/localtime (binary file, copy of a file in /usr/share/zoneinfo)

    tzselect
    locale
    iconv

System time

http://www.pool.ntp.org/en/

    date (display local time)
    hwclock (check or set hardware clock)
    ntp (service to query internet time servers and set local time)

System logging

syslog, rsyslog, systemd journal logging

Facilities & priorities - standard labeling for log messages

rsyslog configuration:

    /etc/rsyslog.d/50-default.conf
    /etc/logrotate.conf
    /etc/logrotate.d/<application> (app-specific logrotate config)

Mail transfer basics

sendmail (original, many other apps have an emulation command), qmail, exim, postfix.

Aliases:

    /etc/aliases (must run newaliases after changing)

Forwarding:

    ~/.forward (file should contain an email or system username)

IP Subnet Masks and CIDR notation

Subnet masks define how much of an IP{ address range a PC needs to keep track of. E.g.:

    192.168.1.[0-255]
    255.255.255.0 (PC only needs to track the final octet number)

CIDR notation counts the ones in the binary notation of the subnet mask:

    255.255.0.0 = /16 (subnet mask binary notation is 11111111.11111111.00000000.00000000)
    255.0.0.0 = /8 (subnet mask binary notation is 11111111.00000000.00000000.00000000)
    255.255.255.0 = /24  (subnet mask binary notation is 11111111.11111111.11111111.00000000)

E.g.:

    192.168.1.10/24 (subnet mask is 255.255.255.0)
    10.16.1.101/16 (subnet mask is 255.255.0.0)

Ports and protocols

Transmission Control Protocol (TCP)

    Very reliable, but slow.

User Datagram Protocol (UDP)

    Much faster than TCP but less reliable (not so much checking)

Internet Control Messaging Protocol (ICMP)

    Not for data transmission, just for checking network connectivity

Ports

    Defines "where" a server listens for particular types of communication
    E.g. port 80 for HTTP requests/responses
    /etc/services (defines what service & protocol listens each each standard port)

Basic network configuration

ifconfig - display information about local network adapter, and loopback adapter. Can also be used for temporary manual network configuration (changes reset).

ipup, ifdown

Config files:

    Debian: /etc/network/interfaces

Basic network troubleshooting

    ifconfig
        Check if network adapters are up
    ping, ping6
        Can we ping a URL?
        Can we ping our gateway?
    Check /etc/resolv.conf
    Check /etc/network/interfaces
        Are we using DHCP or a static IP?
        If static - is gateway defined?
        If static - is dns-nameservers defined?
    traceroute, tracepath

Client-side DNS

Query DNS server:

    dig <DOMAIN>
    dig @<DNS IP ADDRESS> <DOMAIN> (uses specified DN server)

DNS resolution order is defined in /etc/nsswitch.conf (hosts: files dns). This will then use the local /etc/hosts file before using DN servers to resolve a hostname.
Services and security

sysv

    chkconfig servicename off
    sysv-rc-conf servicename off

Upstart

    update-rc.d servicename remove

systemd

    systemctl disable servicename

User limits

ulimit, system-wide limits, current and past logins

    ulimit -a (lists current limit)
    ulimit -f 0 (e.g. set file size limit for current session to 0 bytes)
    /etc/security/limits.conf (set persistent system-wide limits for users and groups)
    who (display users that are logged in)
    w (displays how is logged in and what they are currently running)
    last (displays the last x number of logins)

Encryption and signing

Public private keys:

    Generated together
    File encrypted with PUBLIC KEY can only be decrypted with the PRIVATE KEY.
    File encrypted with PRIVATE KEY can only be decrypted with the PUBLIC KEY.
    Proof of identity: if a file is encrypted using the private key ("signed"), if the file can be decrypted with the public key then it is guaranteed to be from the owner of the private key.

OpenSSH uses keys to confirm identity:

    During the first time connecting, a server's public key is copied into ~/.ssh/known_hosts
    On subsequent connections, the public key is used to decrypt a signed token from the server to verify its identity.

Creating and using keypairs

User SSH keypairs. Generate local key pair and copy public key to remote host .ssh/authorized_keys:

    ssh-keygen (creates id_rsa and id_rsa.pub)
    ssh-copy-id -i ~/.ssh/id_rsa.pub HOSTNAME

Using GPG:

    gpg --gen-key (generate a key)
    gpg --list-key
    gpg --export IDENTITY > FILENAME
    gpg --import FILENAME
    gpg --out OUTPUT_FILE --recipient INDENTITY --encrypt INPUT_FILE (encrypt a file)
    gpg --out OUTPUT_FILE --decrypt INPUT_FILE (decrypt a file)