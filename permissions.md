# Ownerships and Persmissions

> 3 Basic Permissions

Every file and directory has 3 types of permissions:
* `Read r`
* `Write w`
* `Execute x`

> Ownership & Permission Groups

* `User` : (Owner) Person who created the file
* `Group` : Users belonging to a shared group
* `Others` : Everyone else in the system

`ls -l` : See permissions of files/directories present in the current directory in long list format <br>
e.g. `ls -l` produces `drwxrwxr-x 6 vashish vashish 4096 Mar 20 11:29 J1939_AddressClaim` <br> <br>
Following is the meaning - 
* `d` : it is a directory (other options are : `r`-Regular File, `l`-Symbolic Link, `c`-Character Device, `b`-Block Device)
* `rwx` : user permissions [read(r) write(w) execute(x)]
* `rwx` : group permissions [read(r) write(w) execute(x)]
* `r-x` : other permissions [read(r) No-Write(-) execute(x)]
* `6` : 2 + number of immediate subdirectories. The first 2 directories are . and ..
* `vashish` : owner
* `vashish` : group
* `4096` : Size in bytes
* `Mar 20 11:29` : Date time of folder
* `J1939_AddressClaim` : Folder name

**Octal Representation of Permission** <br>
`rwx` can be represented by `111` = 7 <br>
other examples <br>
* `r-x` = `101` = 5
* `--x` = `001` = 1
* `rw-` = `110` = 6

<br>

> Change permissions

`chmod ugo+rwx xyz.txt` : add permissions (+) for user, group & others (ugo) read, write and execute permissions (rwx) <br>
`chmod ug+rw,o-x abc.mp4` : add read and write for user and group, and remove (-) execute for others
<br>

> Detailed specifics of a file

`stat file.txt` <br>
```
File: file.txt
Size: 1024            Blocks: 8          IO Block: 4096   regular file
Device: 801h/2049d    Inode: 1234567     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/ashishman)   Gid: ( 1000/ashishman)
Access: 2026-09-11 10:15:32.123456789 +0530
Modify: 2026-09-11 10:10:05.987654321 +0530
Change: 2026-09-11 10:10:05.987654321 +0530
Birth: 2026-09-11 10:00:00.000000000 +0530
```
`Uid: ( 1000/ashishman)` : Is the unique identifier of user account <br>
`Gid: ( 1000/ashishman)` : Is the unique identifier of group that the user belongs to <br>
`Device: 801h/2049d` : Hexadecimal device number / decimal device number
`Access: (0644/-rw-r--r--)` : Octal and direct representation of accesses
<br>
```
Note: Two files can have same Inodes.
A file is identified by deviceNumber/Inode.
This file.txt is a file stored in filesystem 2049 with a Inode numbered 1234567
```
<br>

> Special permissions

* `chmod u+s program` : set userid permission allows to execute programs with privilege of owner <br>
  before `-rwxr-xr-x root root myprogram` --> `chmod 4755 myprogram` --> after `-rwsr-xr-x root root myprogram` <br>
  for user `rwx` becomes `rws`. 
  

















