## fdisk
> 分区工具:最大分区空间仅支持到2T



### 为redhat添加新磁盘
#### 查看是否有新磁盘
##### 未添加磁盘
$#: ls /dev/sd*
/dev/sda /dev/sda1
##### 添加磁盘
$#: ls /dev/sd*
/dev/sda /dev/sda1 /dev/sdb
#### 创建分区
1、 显示分区
```shell
$#: su -
$#: fdisk /dev/sdb
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0xd1082b01.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
         switch off the mode (command 'c') and change display units to
         sectors (command 'u').

Command (m for help):
Command (m for help): c
DOS Compatibility flag is not set
Command (m for help): u
Changing display/entry units to sectors
Command (m for help): p

Disk /dev/sdb: 34.4 GB, 34359738368 bytes
255 heads, 63 sectors/track, 4177 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xd1082b01

   Device Boot      Start         End      Blocks   Id  System

```  

2、 创建新分区  

```shell
Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4): 1
First sector (2048-67108863, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-67108863, default 67108863):
Using default value 67108863
```
3、 保存并退出
```shell
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```
4、查看新建的分区
```shell
# ls /dev/sd*
/dev/sda  /dev/sda1  /dev/sda2  /dev/sdb  /dev/sdb1
```  
#### 创建文件系统
```shell
# /sbin/mkfs.ext4 -L /backup /dev/sdb1
mke2fs 1.41.12 (17-May-2010)
Filesystem label=/backup
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
2097152 inodes, 8388352 blocks
419417 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
256 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624

Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 36 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.
```
#### 挂载文件系统
mkdir /backup
##### 临时挂载
mount /dev/sdb1 /backup
##### 永久挂载
vi /etc/fstab
```shell
/dev/sdb1  /backup     ext4 default 1 2
or
LABEL=/backup /backup  ext4 default 1 2
```
