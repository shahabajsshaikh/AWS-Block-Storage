# AWS-Block Storage

## Type:

1. EBS
2. EFS
3. Temporary Storage (Instance Storage)

## Create & attched EBS volume to EC2:

```sh
[root@node1 ec2-user]# fdisk -l
Disk /dev/xvda: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd00a186f

Device     Boot Start      End  Sectors Size Id Type
/dev/xvda1       2048     4095     2048   1M 83 Linux
/dev/xvda2 *     4096 20971486 20967391  10G 83 Linux
```
* After affach new 1 GB EBS to ec2
```sh
[root@node1 ec2-user]# lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  10G  0 disk
├─xvda1 202:1    0   1M  0 part
└─xvda2 202:2    0  10G  0 part /
xvdf    202:80   0   1G  0 disk  <=================== New volume attached

[root@node1 ec2-user]# fdisk -l
Disk /dev/xvda: 10 GiB, 10737418240 bytes, 20971520 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xd00a186f

Device     Boot Start      End  Sectors Size Id Type
/dev/xvda1       2048     4095     2048   1M 83 Linux
/dev/xvda2 *     4096 20971486 20967391  10G 83 Linux

Disk /dev/xvdf: 1 GiB, 1073741824 bytes, 2097152 sectors <=================== New volume attached
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

```
* Format attached volume.

```sh
[root@node1 ec2-user]# mkfs.ext4 /dev/xvdf
mke2fs 1.45.4 (23-Sep-2019)
Creating filesystem with 262144 4k blocks and 65536 inodes
Filesystem UUID: 7f6e4bbc-08d3-4221-92d3-53a9cf4fd854
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376

Allocating group tables: done
Writing inode tables: done
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done
```
* Create file and mount in new volume
```sh
[root@node1 ec2-user]# vi index.html

[root@node1 ec2-user]# pwd
/home/ec2-user

[root@node1 ec2-user]# ls
index.html

[root@node1 ec2-user]# mount /dev/xvdf /home/ec2-user/  <==== mount

```
- Mount permonently 
```sh
[root@node1 etc]# vim /etc/fstab

/dev/xvdf /home/ec2-user/ ext4 defults  0 0  <=== line in end of 

[root@node1 etc]# mount -a

[root@node1 etc]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        386M     0  386M   0% /dev
tmpfs           408M     0  408M   0% /dev/shm
tmpfs           408M   21M  388M   6% /run
tmpfs           408M     0  408M   0% /sys/fs/cgroup
/dev/xvda2       10G  1.8G  8.3G  18% /
tmpfs            82M     0   82M   0% /run/user/1000
/dev/xvdf       976M  2.6M  907M   1% /home/ec2-user <========= Checked 
```
- Confim by add files in local mount location:

```sh
[root@node1 ec2-user]# touch test{0..100} <===== create 100 file at location /home/ec2-user

[root@node1 ec2-user]# ls
lost+found  test13  test2   test26  test32  test39  test45  test51  test58  test64  test70  test77  test83  test9   test96
test0       test14  test20  test27  test33  test4   test46  test52  test59  test65  test71  test78  test84  test90  test97
test1       test15  test21  test28  test34  test40  test47  test53  test6   test66  test72  test79  test85  test91  test98
test10      test16  test22  test29  test35  test41  test48  test54  test60  test67  test73  test8   test86  test92  test99
test100     test17  test23  test3   test36  test42  test49  test55  test61  test68  test74  test80  test87  test93
test11      test18  test24  test30  test37  test43  test5   test56  test62  test69  test75  test81  test88  test94
test12      test19  test25  test31  test38  test44  test50  test57  test63  test7   test76  test82  test89  test95

```
## Dettached EBS volume from one node1 to another node2 & attched EBS volume to EC2:
```sh
[root@node2 ec2-user]# fdisk -l
Disk /dev/xvda: 8 GiB, 8589934592 bytes, 16777216 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 66B3909F-969E-4FD1-901C-CEE3A9974A83

Device       Start      End  Sectors Size Type
/dev/xvda1    4096 16777182 16773087   8G Linux filesystem
/dev/xvda128  2048     4095     2048   1M BIOS boot

Partition table entries are not in disk order.


Disk /dev/xvdf: 1 GiB, 1073741824 bytes, 2097152 sectors <==== Attached volume from node1 to node2
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```
