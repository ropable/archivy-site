---
date: 02-09-21
id: 29
path: Courses
tags: []
title: Linux LPIC-2 exam notes
type: note
---


Monitor CPU & memory usage

Tools:

    vmstat (basic system info)
    free (memory info, use -m to report output in megabytes)
    top (press 1 to expand CPUs)
    stress (can be used to create synthetic load)

Monitor disk I/O

System load (via top/htop/uptime):

    1 CPU: 0.5 (50% used), 1.0 (100% used)
    4 CPU: 0.5 (12.5% used), 1.0 (25% used), 5.0 (125% used)

uptime: shows uptime and system load - three numbers (last 1 minute, last 5 minutes, last 15 minutes).

I/O wait:

    Percentage of CPU actions waiting for disk access.
    Process is I/O blocked, and in "uninterruptible sleep" (can't be killed!)

top: shows I/O wait (look for: x.x wa).

iotop: specifically for disk I/O.

iostat, sar (install via sysstat on Debian) - older tools, use iotop instead.

lsof: show open/in-use files.
Monitoring network I/O

Identify network interfaces:

    ifconfig
    netstat -ie (interfaces)
    netstat -s (statistics)
    netstat -tuna (lots of good info)

Tools:

    iftop
    nload (graph of bandwidth)

Monitoring bandwidth

Tools:

    iperf
    speedtest (https://github.com/sivel/speedtest-cli)

Monitoring data trends

collectd - daemon for collecting statistics about a system.

Edit /etc/collectd.conf to configure.

Collectd will include a collection.cgi that can provide a basic web interface of the data.
Kernel components

Kernel version number example: 2.6.3.14 (version.major_revision.minor_revision.patch).

Kernel information: uname -a

Also review /boot to see kernel boot discs. Kernel-specific modules can be found in /lib/modules/<KERNEL_VERSION>
Manipulating kernel modules

List installed modules: lsmod

Inside /lib/modules/<KERNEL_VERSION>, find a module: find . | grep <module name>

Insert/remove a module: insmod, rmmod

A better tool: modprobe

Insert a module: modprobe <name>

Remove a module: modprobe -r <name>

Module info: modinfo <name>
Kernel automation and configuration

/sbin/sysctl can set kernel settings manually.

Show the current kernel settings: sysctl -a

Live kernel settings: /proc/sys/kernel

To make kernel configuration persistent: /etc/sysctl.conf

Applications will install kernel settings in /etc/sysctld.d/

NOTE: sysctl.conf overrides /etc/sysctld.d/

udev monitors the system for changes and inserts/removes modules as your system needs them.

udevadm monitor (udevmonitor on older systems) - real time monitoring of udev activities.
Compiling the kernel: the tools

Developer programs (Debian): sudo apt-get install build-essential ncurses-devel qt-devel

Standard location for a custom kernel: /usr/src/linux

Acquiring the source: https://www.kernel.org

Extract it into the /usr/src directory: tar -Jxvf <archive filename> /usr/src

Create a symlink from the extracted kernel to /usr/src/linux

Kernel documentation: /usr/src/linux/Documentation
Kernel compile: compilation and installation

Make targets:

    make help
    make mrproper (cleans up old compile files)
    make menuconfig (menu-based configuration for the kernel)
    make bzImage (make big zimage and compile the kernel, long step)
    make modules (compiles kernel modules, longest step)
    make modules_install (takes compiled modules and copies them to the /lib/modules directory)
    make install (writes entries to bootloader to boot our compiled kernel)

Module options

Dracut
System V (SysV) runlevels

Being outdated by Upstart and systemd, but still very common.

See /etc/inittab for config.

Use telinit X to change runlevel.
System V init scripts

/etc/init.d: location for startup/stop scripts for different services and applications.

/etc/rc.d: location for scripts that run on startup to different runlevels.

chkconfig (CentOS)

update-rc.d (Debian, Ubuntu)