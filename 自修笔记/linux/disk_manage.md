
## 使用一块新磁盘分三步

### 磁盘分区
```
[root@k8s-master ~]# fdisk -l

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sda: 32.2 GB, 32212254720 bytes, 62914560 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a6f21

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048      616447      307200   83  Linux
/dev/sda2          616448     4810751     2097152   82  Linux swap / Solaris
/dev/sda3         4810752    41943039    18566144   83  Linux



[root@k8s-master ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        18G  4.0G   14G  23% /
devtmpfs        898M     0  898M   0% /dev
tmpfs           912M   84K  912M   1% /dev/shm
tmpfs           912M  9.0M  903M   1% /run
tmpfs           912M     0  912M   0% /sys/fs/cgroup
/dev/sda1       297M  152M  146M  51% /boot
tmpfs           183M   12K  183M   1% /run/user/42
tmpfs           183M     0  183M   0% /run/user/0



```
> 可见sdb没有进行磁盘分区，所以下面没有列出sdb1..
> df -h 也查看不到这块磁盘。

```
[root@k8s-master ~]# fdisk /dev/sdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x2eb87ba3.

Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)

Command (m for help): p

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-20971519, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): +3G
Partition 1 of type Linux and of size 3 GiB is set

Command (m for help): p

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     6293503     3145728   83  Linux

Command (m for help): n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): e
Partition number (2-4, default 2): 2
First sector (6293504-20971519, default 6293504):
Using default value 6293504
Last sector, +sectors or +size{K,M,G} (6293504-20971519, default 20971519):
Using default value 20971519
Partition 2 of type Extended and of size 7 GiB is set

Command (m for help): p

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     6293503     3145728   83  Linux
/dev/sdb2         6293504    20971519     7339008    5  Extended

Command (m for help): n
Partition type:
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): l
Adding logical partition 5
First sector (6295552-20971519, default 6295552):
Using default value 6295552
Last sector, +sectors or +size{K,M,G} (6295552-20971519, default 20971519): _^H^H+3G
Last sector, +sectors or +size{K,M,G} (6295552-20971519, default 20971519): +3G
Partition 5 of type Linux and of size 3 GiB is set

Command (m for help): p

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     6293503     3145728   83  Linux
/dev/sdb2         6293504    20971519     7339008    5  Extended
/dev/sdb5         6295552    12587007     3145728   83  Linux

Command (m for help): t
Partition number (1,2,5, default 5): 5
Hex code (type L to list all codes): L

 0  Empty           24  NEC DOS         81  Minix / old Lin bf  Solaris
 1  FAT12           27  Hidden NTFS Win 82  Linux swap / So c1  DRDOS/sec (FAT-
 2  XENIX root      39  Plan 9          83  Linux           c4  DRDOS/sec (FAT-
 3  XENIX usr       3c  PartitionMagic  84  OS/2 hidden C:  c6  DRDOS/sec (FAT-
 4  FAT16 <32M      40  Venix 80286     85  Linux extended  c7  Syrinx
 5  Extended        41  PPC PReP Boot   86  NTFS volume set da  Non-FS data
 6  FAT16           42  SFS             87  NTFS volume set db  CP/M / CTOS / .
 7  HPFS/NTFS/exFAT 4d  QNX4.x          88  Linux plaintext de  Dell Utility
 8  AIX             4e  QNX4.x 2nd part 8e  Linux LVM       df  BootIt
 9  AIX bootable    4f  QNX4.x 3rd part 93  Amoeba          e1  DOS access
 a  OS/2 Boot Manag 50  OnTrack DM      94  Amoeba BBT      e3  DOS R/O
 b  W95 FAT32       51  OnTrack DM6 Aux 9f  BSD/OS          e4  SpeedStor
 c  W95 FAT32 (LBA) 52  CP/M            a0  IBM Thinkpad hi eb  BeOS fs
 e  W95 FAT16 (LBA) 53  OnTrack DM6 Aux a5  FreeBSD         ee  GPT
 f  W95 Ext'd (LBA) 54  OnTrackDM6      a6  OpenBSD         ef  EFI (FAT-12/16/
10  OPUS            55  EZ-Drive        a7  NeXTSTEP        f0  Linux/PA-RISC b
11  Hidden FAT12    56  Golden Bow      a8  Darwin UFS      f1  SpeedStor
12  Compaq diagnost 5c  Priam Edisk     a9  NetBSD          f4  SpeedStor
14  Hidden FAT16 <3 61  SpeedStor       ab  Darwin boot     f2  DOS secondary
16  Hidden FAT16    63  GNU HURD or Sys af  HFS / HFS+      fb  VMware VMFS
17  Hidden HPFS/NTF 64  Novell Netware  b7  BSDI fs         fc  VMware VMKCORE
18  AST SmartSleep  65  Novell Netware  b8  BSDI swap       fd  Linux raid auto
1b  Hidden W95 FAT3 70  DiskSecure Mult bb  Boot Wizard hid fe  LANstep
1c  Hidden W95 FAT3 75  PC/IX           be  Solaris boot    ff  BBT
1e  Hidden W95 FAT1 80  Old Minix
Hex code (type L to list all codes): 82
Changed type of partition 'Linux' to 'Linux swap / Solaris'

Command (m for help): p

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     6293503     3145728   83  Linux
/dev/sdb2         6293504    20971519     7339008    5  Extended
/dev/sdb5         6295552    12587007     3145728   82  Linux swap / Solaris

Command (m for help): n
Partition type:
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): p
Partition number (3,4, default 3):
No free sectors available

Command (m for help): n
Partition type:
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): l
Adding logical partition 6
First sector (12589056-20971519, default 12589056):
Using default value 12589056
Last sector, +sectors or +size{K,M,G} (12589056-20971519, default 20971519):
Using default value 20971519
Partition 6 of type Linux and of size 4 GiB is set

Command (m for help): p

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     6293503     3145728   83  Linux
/dev/sdb2         6293504    20971519     7339008    5  Extended
/dev/sdb5         6295552    12587007     3145728   82  Linux swap / Solaris
/dev/sdb6        12589056    20971519     4191232   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.

```
> 分区好后的磁盘仍然可以输入t修改其作用<br/>
> 完成后要输入w写入，或者输入q放弃分区。

```
[root@k8s-master ~]# fdisk -l

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     6293503     3145728   83  Linux
/dev/sdb2         6293504    20971519     7339008    5  Extended
/dev/sdb5         6295552    12587007     3145728   82  Linux swap / Solaris
/dev/sdb6        12589056    20971519     4191232   83  Linux

Disk /dev/sda: 32.2 GB, 32212254720 bytes, 62914560 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a6f21

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048      616447      307200   83  Linux
/dev/sda2          616448     4810751     2097152   82  Linux swap / Solaris
/dev/sda3         4810752    41943039    18566144   83  Linux
[root@k8s-master ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        18G  4.0G   14G  23% /
devtmpfs        898M     0  898M   0% /dev
tmpfs           912M   84K  912M   1% /dev/shm
tmpfs           912M  9.0M  903M   1% /run
tmpfs           912M     0  912M   0% /sys/fs/cgroup
/dev/sda1       297M  152M  146M  51% /boot
tmpfs           183M   16K  183M   1% /run/user/42
tmpfs           183M     0  183M   0% /run/user/0

```
> 可见sdb已被分区，但是由于没有制作文件系统和挂载，所以所以df -h 仍然看不到这块磁盘




### 制作文件系统
```
[root@k8s-master ~]# blkid
/dev/sda1: UUID="d847315c-72b1-4fda-ab20-6ea72960de39" TYPE="xfs"
/dev/sda2: UUID="c4d4888c-cff3-49f5-8b44-e473d86ed172" TYPE="swap"
/dev/sda3: UUID="c3b7150e-680c-4bdb-a9c5-f47ad57e4a14" TYPE="xfs"

[root@k8s-master ~]# mkfs -t xfs /dev/sdb1
meta-data=/dev/sdb1              isize=512    agcount=4, agsize=196608 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=786432, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
[root@k8s-master ~]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        18G  4.0G   14G  23% /
devtmpfs        898M     0  898M   0% /dev
tmpfs           912M   84K  912M   1% /dev/shm
tmpfs           912M  9.0M  903M   1% /run
tmpfs           912M     0  912M   0% /sys/fs/cgroup
/dev/sda1       297M  152M  146M  51% /boot
tmpfs           183M   16K  183M   1% /run/user/42
tmpfs           183M     0  183M   0% /run/user/0

[root@k8s-master ~]# fdisk -l

Disk /dev/sdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x2eb87ba3

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048     6293503     3145728   83  Linux
/dev/sdb2         6293504    20971519     7339008    5  Extended
/dev/sdb5         6295552    12587007     3145728   82  Linux swap / Solaris
/dev/sdb6        12589056    20971519     4191232   83  Linux

Disk /dev/sda: 32.2 GB, 32212254720 bytes, 62914560 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000a6f21

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048      616447      307200   83  Linux
/dev/sda2          616448     4810751     2097152   82  Linux swap / Solaris
/dev/sda3         4810752    41943039    18566144   83  Linux
[root@k8s-master ~]# blkid
/dev/sda1: UUID="d847315c-72b1-4fda-ab20-6ea72960de39" TYPE="xfs"
/dev/sda2: UUID="c4d4888c-cff3-49f5-8b44-e473d86ed172" TYPE="swap"
/dev/sda3: UUID="c3b7150e-680c-4bdb-a9c5-f47ad57e4a14" TYPE="xfs"
/dev/sdb1: UUID="1433ffe3-0404-4525-b544-c444c13749ac" TYPE="xfs"


```





### 挂载
```
[root@k8s-master ~]# man mount
[root@k8s-master ~]# mount -t xfs /dev/sdb1 /mnt/zhaweiwei
mount: mount point /mnt/zhaweiwei does not exist
[root@k8s-master ~]# mkdir /mnt/zhaweiwei
[root@k8s-master ~]# mount -t xfs /dev/sdb1 /mnt/zhaweiwei
[root@k8s-master ~]# cd /mnt
[root@k8s-master mnt]# ls
zhaweiwei

[root@k8s-master mnt]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda3        18G  4.0G   14G  23% /
devtmpfs        898M     0  898M   0% /dev
tmpfs           912M   84K  912M   1% /dev/shm
tmpfs           912M  9.0M  903M   1% /run
tmpfs           912M     0  912M   0% /sys/fs/cgroup
/dev/sda1       297M  152M  146M  51% /boot
tmpfs           183M   16K  183M   1% /run/user/42
tmpfs           183M     0  183M   0% /run/user/0
/dev/sdb1       3.0G   33M  3.0G   2% /mnt/zhaweiwei

```
> mount -t xfs 这里的xfs要和上面制作的文件系统对应起来





















---
#
#
<meta http-equiv="refresh" content="5">