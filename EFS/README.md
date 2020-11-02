# EFS Amazon-Elastic File System.
### Useful-links
| Details | URL |
| ------ | ------ |
| EFS-Setup | https://www.youtube.com/watch?v=ZVCCjSGPRUA |

## EFS via NFS
### Step 1:
Create EFS with required AZ's.

### Step 2:
Install NFS on two ec2's (Redhat8).
```
 yum update -y && yum install nfs-utils -y
 ```
 Check status and start the service.
 ```
 service nfs-utils status && service nfs-utils start
 ```
 ### Step 3:
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
Create folder for mount at node 1 and same way create different folder at node 2.
#### Node 1.
```
[root@ip-172-31-0-69 ec2-user]# mkdir mount1

[root@ip-172-31-0-69 ec2-user]# cd mount1/

[root@ip-172-31-0-69 mount1]# pwd
/home/ec2-user/mount1

[root@ip-172-31-0-69 mount1]# ls
demo.html

[root@ip-172-31-0-69 mount1]# mount -t nfs4 fs-d3f09e02.efs.ap-south-1.amazonaws.com:/ /mnt/demo

```
Check and verify mount the folder or not and create file index.html should reflected at 2nd ec2 mount location.
```
[root@ip-172-31-0-69 mount1]# df -h
Filesystem                                  Size  Used Avail Use% Mounted on
devtmpfs                                    386M     0  386M   0% /dev
tmpfs                                       408M     0  408M   0% /dev/shm
tmpfs                                       408M   21M  388M   6% /run
tmpfs                                       408M     0  408M   0% /sys/fs/cgroup
/dev/xvda2                                   10G  1.8G  8.3G  18% /
tmpfs                                        82M     0   82M   0% /run/user/1000
fs-d3f09e02.efs.ap-south-1.amazonaws.com:/  8.0E     0  8.0E   0% /mnt/demo
```
#### Node 2.
```
[root@ip-172-31-0-69 ec2-user]# mkdir replica

[root@ip-172-31-0-69 ec2-user]# mount -t nfs4 fs-d3f09e02.efs.ap-south-1.amazonaws.com:/ /home/ec2-user/replica

[root@ip-172-31-0-69 replica]# df -h
Filesystem                                  Size  Used Avail Use% Mounted on
devtmpfs                                    474M     0  474M   0% /dev
tmpfs                                       492M     0  492M   0% /dev/shm
tmpfs                                       492M  408K  492M   1% /run
tmpfs                                       492M     0  492M   0% /sys/fs/cgroup
/dev/xvda1                                  8.0G  1.6G  6.4G  20% /
tmpfs                                        99M     0   99M   0% /run/user/1000
fs-d3f09e02.efs.ap-south-1.amazonaws.com:/  8.0E     0  8.0E   0% /home/ec2-user/replica

[root@ip-172-31-0-69 replica]# ls
demo.html
```
