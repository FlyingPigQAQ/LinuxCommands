## parted
> 服务器创建2T以上的分区需要使用GPT分区  
### GUID Partition Table Scheme
https://www.cyberciti.biz/media/new/tips/2007/11/guid-partition-table-schemesvg.png
### 先决条件
Include GPT support in the kernel (Set CONFIG_EFI_PARTITION to y to compile this feature)
***Please note that almost all new kernel and latest distro supports GPT***
### 查看当前磁盘空间大小
fdisk -l /dev/sdb
```shell
Disk /dev/sdb: 3000.6 GB, 3000592982016 bytes
255 heads, 63 sectors/track, 364801 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

Disk /dev/sdb doesn't contain a valid partition table
```  

###  创建3TB分区
1、 create a partition
```shell
$#: parted /dev/sdb
GNU Parted 2.3
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted)
```
2、 create a new GPT disklabel
```shell
(parted) mklabel gpt
Warning: The existing disk label on /dev/sdb will be destroyed and all data on this disk will be lost. Do you want to continue?
Yes/No? yes
(parted)
```
3、 set the default unit to TB
```shell
(parted) unit TB
```
4、 create 3TB partition
```shell
(parted) mkpart primary 0TB 3TB
```
5、 print current Partition
```shell
(parted) print
Model: ATA ST33000651AS (scsi)
Disk /dev/sdb: 3.00TB
Sector size (logical/physical): 512B/512B
Partition Table: gpt

Number  Start   End     Size    File system  Name     Flags
 1      0.00TB  3.00TB  3.00TB  ext4         primary
```
6、 help
```shell
(parted) help
```
7、quit and save Changes
```shell
(parted) quit
Information: You may need to update /etc/fstab.
```
### 创建ext4文件系统
```shell
$#: mkfs.ext4 -L /backup /dev/sdb1

mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
183148544 inodes, 732566272 blocks
36628313 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
22357 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968,
	102400000, 214990848, 512000000, 550731776, 644972544

Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 31 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.
```
### 替代fdisk命令，gdisk
