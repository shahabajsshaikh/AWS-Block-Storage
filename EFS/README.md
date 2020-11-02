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
