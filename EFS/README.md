# EFS Amazon-Elastic File System.

### Useful-links
| Details | URL |
| ------ | ------ |
| EFS-Setup | https://www.youtube.com/watch?v=ZVCCjSGPRUA |

### Step 1:
Create EFS with required AZ's.

### Step 2:
Install NFS on ec2 (Redhat8)
```
 yum update -y && yum install nfs-utils -y
 ```
 Check status and start the service.
 ```
 service nfs-utils status && service nfs-utils start
 ```
 Before unmount.
 ```
 [root@ip-172-31-47-112 mnt]# df -h
Filesystem      Size  Used Avail Use% Mounted on
devtmpfs        386M     0  386M   0% /dev
tmpfs           408M     0  408M   0% /dev/shm
tmpfs           408M   21M  388M   6% /run
tmpfs           408M     0  408M   0% /sys/fs/cgroup
/dev/xvda2       10G  1.8G  8.3G  18% /
tmpfs            82M     0   82M   0% /run/user/1000
```
Create folder for mount.
```
[root@ip-172-31-47-112 mount1]# cd /mnt/

[root@ip-172-31-47-112 mnt]# mkdir demo

[root@ip-172-31-47-112 mnt]# ls
demo

[root@ip-172-31-47-112 mnt]# mount -t nfs4 fs-d3f09e02.efs.ap-south-1.amazonaws.com:/ /mnt/demo

```
Check and verify mount the folder or not.
```
[root@ip-172-31-47-112 mnt]# df -h
Filesystem                                  Size  Used Avail Use% Mounted on
devtmpfs                                    386M     0  386M   0% /dev
tmpfs                                       408M     0  408M   0% /dev/shm
tmpfs                                       408M   21M  388M   6% /run
tmpfs                                       408M     0  408M   0% /sys/fs/cgroup
/dev/xvda2                                   10G  1.8G  8.3G  18% /
tmpfs                                        82M     0   82M   0% /run/user/1000
fs-d3f09e02.efs.ap-south-1.amazonaws.com:/  8.0E     0  8.0E   0% /mnt/demo
```
