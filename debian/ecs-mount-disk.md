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

## 6 数据盘容量用尽后的扩容方案

开始买的独立数据盘的容量是 20G, 最近几天用尽，带来的结果是上传附件时出现如下错误：

> mkdir(): No space left on device

### 思路一 扩容现有数据盘【已实践】

首先想到的是把现有数据盘进行扩容。控制台倒是有一个“扩容”操作，但该操作仅扩大磁盘容量，文件系统的容量并未改变。拿自己的数据盘为例，初始容量是 20G, 扩容后，磁盘容量是 40G, 但是文件系统容量仍是 20G, 也就是说，照样存不进去内容。只有同时把文件系统也扩展到 40G 才行。但是扩展文件系统意味着必须格式化原内容，这样数据丢失风险太大。简单用了一下快照也不行，只能使用最笨的办法：使用 FTP 把数据盘备份到本地，然后格式化数据盘，使得文件系统也得到扩容，最后再把本地的数据上传到扩容后的数据盘上。这么做太耗时，线上应用根本等不及。

### 思路二 新购一块数据盘

用文章开头的办法再买一块单独的磁盘，挂载到另一个目录下。新上传的附件直接传到此数据盘内。**这里的关键是应用如何知道该从那一块数据盘内读写数据**。简单思考了一下，不难实现：所有附件都是通过通用模型 Media 控制的。我们可以使用现有 media 表内最后一条记录的 id 或者创建时间戳作为判断依据，大致逻辑如下：

```php
// in generic Media model
public function getMediaRoot()
{
    /**
     * 这里进行判断。file 和 static 两个 aliases 分别代表旧新两个数据盘挂载的目录
     * 新增记录（写操作）一律使用新磁盘； media ID 大于 10324 的表示在新盘上存储，否则在旧磁盘上存储
     */
    if ($this->isNewRecord || $this->id > 10324) {
        return Yii::getAlias('@static');
    }

    return Yii::getAlias('@file');
}
```

将来如果 `static` 磁盘也用尽，我们可以使用类似的办法再增加数据盘。

### Update 2024-03-22 第二次扩容

执行 `growpart` 先后两次的变化：

```
CHANGED: partition=1 start=2048 old: size=83884032 end=83886080 new: size=125827039,end=125829087
CHANGED: partition=1 start=2048 old: size=125827039 end=125829087 new: size=188741599,end=188743647
```

执行 `resize2fs` 前后的变化：

```
old_desc_blocks = 3, new_desc_blocks = 4
The filesystem on /dev/vdc1 is now 15728379 (4k) blocks long
old_desc_blocks = 4, new_desc_blocks = 6
The filesystem on /dev/vdc1 is now 23592699 (4k) blocks long.
```

### Update 2023-03-11 扩容现有云盘

之前新购的 40G 云盘已用完，再次面临扩容问题。查看相关文档后，决定尝试上面的思路一实现。以数据盘为例，扩容主要分为三个步骤：

1. 扩容云盘；
2. 扩容分区；
3. 扩容文件系统；

上一次只进行了步骤一，后两个步骤因为怕麻烦没有实践。这次实践后发现还是思路一更好。

#### Step 1: 创建快照备份

以防万一,创建一个只保留一天的快照。快照创建后还有一个短暂的上传时间。需要稍等一下才能进行第二步。

#### Step 2: 扩容云盘

在控制台上找到数据盘，点击扩容即可。每次扩容 20G 足矣。

#### Step 3: 扩容分区

首先，通过 `sudo fdisk -lu /dev/vdb` 查看要扩容的云盘概况：

```
git@YALONGDIAMOND:~$ sudo fdisk -lu /dev/vdc
Disk /dev/vdc: 90 GiB, 96636764160 bytes, 188743680 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x08966605

Device     Boot Start       End   Sectors Size Id Type
/dev/vdc1        2048 188743646 188741599  90G 83 Linux
# 上一行最后侧的 'Linux' 表明数据盘使用的是MBR分区表格式

```

2 安装**growpart** 包:

```
sudo apt-get update
sudo apt-get install cloud-guest-utils
```

> `sudo LC_ALL=en_US.UTF-8 growpart /dev/vdc 1`. 

```
# 注意需使用 sudo 执行。
git@YALONGDIAMOND:~/www/eims$ LC_ALL=en_US.UTF-8 growpart /dev/vdc 1
FAILED: sfdisk not found

git@YALONGDIAMOND:~/www/eims$ sudo LC_ALL=en_US.UTF-8 growpart /dev/vdc 1
CHANGED: partition=1 start=2048 old: size=83884032 end=83886080 new: size=125827039,end=125829087
```

最后，执行文件分区扩容命令(`ext*` 格式)

> `sudo resize2fs /dev/vdc1`

```
git@YALONGDIAMOND:~/www/eims$ sudo resize2fs /dev/vdc1
resize2fs 1.43.4 (31-Jan-2017)
Filesystem at /dev/vdc1 is mounted on /home/git/www/eims/static; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 4
The filesystem on /dev/vdc1 is now 15728379 (4k) blocks long.
```

大功告成。两次扩容实践一对比，高下立判。

## 参考:

- [Linux 系统挂载数据盘](http://help.aliyun.com/knowledge_detail.htm?knowledgeId=5974154) 阿里云官方文档
- [扩展分区和文件系统 Linux数据盘](https://help.aliyun.com/document_detail/25452.html?spm=a2c4g.11186623.2.25.73564656IqGe03#concept-z11-xsh-ydb) 阿里云官方文档

