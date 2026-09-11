# Directory structures
1. `System Binaries`
2. `Boot and Kernel Files`
3. `Configuration Files`
4. `User related`
5. `Shared Libraries`
6. `Mount and Media`
7. `System info`
8. `Multi user resources`
9. `Temp files`
10. `Optional Software`

> ROOT

Root directory is the top of the chain denoted by `/` <br>
/root and / are not same <br>
/root is a sub-directory of /. It is a personal directory of root user <br>

> /bin

utilities are located here <br>
e.g. `ls`, `cp`, `mv`, `cat` etc

> /sbin

critical commands for system administration <br>
mount fsck <br>
shutdown

> /lib , /lib32 and /lib64

Shared libraries (like various dlls in windows)
Kernel modules <br>

> /usr

unix system resources <br>
many installed applications live here <br>
usr mirrors many other directories e.g. `/usr/lib`, `/usr/bin`, `/usr/sbin` <br>

> /boot

booting related files

* initrd
* initramfs
* GRUB etc

> /dev

device files

* block devices
* character devices

> /etc

editable text configuration <br>
system wide configurations <br>

> /home

personal directory for users <br>
/home/emma <br>
/home/joe

`note - .bashrc, .file is a way to create hidden files`
`root user has a separate home called /root`

> /media

Desigened for removable devices, usb, cd <br>
/dev holds files for controlling the devices, /media is where the devices are mounted

> /mnt

mounting file systems <br>
unlike /media, manual mount is done in /mnt. /media mounts devices automatically

> /proc

virtual file system to show the status of the system <br>
monitor debug and interact linux <br>
/proc/1234 <br>
/proc/cpuinfo <br>
/proc/meminfo

> /sys

system utilities to change kernel configurations <br>
driver dev time

> /run

temporary file storage that evaporates on reboot <br>

> /srv

data for services are stored <br>
files for webservers, ftp servers etc 

> /var

variable. <br>
log, cache etc are stored. <br>
/var/log stores temp logs

> /tmp

temp files <br>
has sticky bit set. where files can be renamed or deleted by only owner or root. <br>
tmp lives in ram not in harddisk. so sometimes is rotated to keep memory free.

> /opt

optional and 3rd party software installed <br>
e.g. /opt/google/chrome  <br>
safe space where 3rd party software can be installed.















