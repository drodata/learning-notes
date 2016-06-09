# 挂载阿里云 ECS 独立磁盘

默认的 ECS 仅有一块磁盘，所有数据都安装在上面。为了数据安全，我们需要单独购买一块数据盘用来存放数据，和存放操作系统的系统盘分开。下面简单介绍挂载过程。

## 1 查看独立磁盘信息 `fdisk -l` 

```bash
$ sudo fdisk -l

Disk /dev/xvda: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders, total 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00068823

    Device Boot      Start         End      Blocks   Id  System
/dev/xvda1   *        2048    41940991    20969472   83  Linux

Disk /dev/xvdb: 10.7 GB, 10737418240 bytes
255 heads, 63 sectors/track, 1305 cylinders, total 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000

Disk /dev/xvdb doesn't contain a valid partition table
```

磁盘 `/dev/xvdb` 正是自己购买的独立磁盘。

## 2 `fdisk /dev/xvdb` 添加分区

```
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x6b31cd47.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help):
```

按下 `m` 显示帮助信息：

```
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
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
```

按下 `n`, 添加一个新分区：

```
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p):
```

选择分区类型，我们使用默认的 `p`:

```
Select (default p): p
Partition number (1-4, default 1): 
```

选择分区数量，仍然使用默认值 `1`, 即用整个磁盘只分一个区:

```
Partition number (1-4, default 1): 1
First sector (2048-20971519, default 2048): 
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): 
Using default value 20971519

Command (m for help):
```

输入 `wq`, 将分区表写入磁盘并退出：

```
Command (m for help): wq
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

再次 `fdisk -l` 查看磁盘信息如下：

```
Disk /dev/xvda: 21.5 GB, 21474836480 bytes
255 heads, 63 sectors/track, 2610 cylinders, total 41943040 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00068823

    Device Boot      Start         End      Blocks   Id  System
/dev/xvda1   *        2048    41940991    20969472   83  Linux

Disk /dev/xvdb: 10.7 GB, 10737418240 bytes
107 heads, 17 sectors/track, 11529 cylinders, total 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x6b31cd47

    Device Boot      Start         End      Blocks   Id  System
/dev/xvdb1            2048    20971519    10484736   83  Linux
```

这表示分区创建成功。

## 3 格式化新建的分区

执行如下命令：

```bash
mkfs.ext3 /dev/xvdb1 
```

命令 `mkfs.ext3` 是创建一个 ext3 格式的文件系统。此命令的输出内容为：

```
mke2fs 1.42.5 (29-Jul-2012)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
655360 inodes, 2621184 blocks
131059 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2684354560
80 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done 
```


## 4 添加分区信息

执行下面命令

```bash
echo '/dev/xvdb1 /home ext3 defaults 0 0' >> /etc/fstab
```

这里我们打算将 `/home/` 目录放在独立磁盘上，如果在独立磁盘上存放其它目录的内容，替换 `/home/` 即可。

执行上面的命令没有任何动静，我们需要通过执行

	cat /etc/fstab

来判断操作是否成功，如果看到如下信息，则表示新分区信息写入工作成功：

```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
UUID=bbbb-xxxx / ext4 errors=remount-ro 0 1
/dev/xvdb1 /home ext3 defaults 0 0
```

## 5 `mount -a` 挂载新分区

`-a` 参数的意思是，把 `/etc/fstab` 中提到的所有文件系统都进行挂载。

再次用 `df -h` 查看，输出如下：

```
Filesystem                                              Size  Used Avail Use% Mounted on
rootfs                                                   20G  946M   18G   5% /
udev                                                     10M     0   10M   0% /dev
tmpfs                                                    51M  212K   50M   1% /run
/dev/disk/by-uuid/bxxx   								 20G  946M   18G   5% /
tmpfs                                                   5.0M     0  5.0M   0% /run/lock
tmpfs                                                   101M     0  101M   0% /run/shm
/dev/xvdb1                                              9.9G  151M  9.2G   2% /home
```

从最后一行可以看出，独立磁盘 `/dev/xvdb1` 已成功挂载到 `/home/` 目录上。至此，独立磁盘挂载工作完成。

## 参考:

- [Linux 系统挂载数据盘](http://help.aliyun.com/knowledge_detail.htm?knowledgeId=5974154) 阿里云官方文档

