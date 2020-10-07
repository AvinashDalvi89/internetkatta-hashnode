## How to restore crash or down AWS EC2 data backup

 Here are the steps for restoring data from aws ec2:

1. First stop ec2 instance
2. detach volume attached to that ec2 instance
3. launch new ec2 or used any existing ec2 running instance
4. attached same volume to new ec2
5. login to new ec2 instance
6. mound old volume in new ec2 using command, so it will behave like external HDD

Hurry 🍻 now you can get data from that volume (all code and database also)
command to run :

```
sudo fdisk -l
mount /dev/sdb1 /media/newhd
``` 
