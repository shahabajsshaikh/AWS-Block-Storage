# AWS-Block Storage

## Type:

1. EBS
2. EFS
3. Temporary Storage (Instance Storage)

```sh
[root@ip-172-31-32-243 ec2-user]# fdisk -l
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
After affach new 1 GB EBS to ec2

```sh
[root@poc dev]# lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  10G  0 disk
├─xvda1 202:1    0   1M  0 part
└─xvda2 202:2    0  10G  0 part /
xvdf    202:80   0   1G  0 disk  <======================================New volume attached
```
