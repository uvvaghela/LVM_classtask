# LVM_classtask
User & Permission Management
Create users: user1, user2, user3
Create group: developers
Add users to the group
Create a shared directory /project
Configure permissions so only group members can access it
**Perform  :**
**	Create 3 user ** : 
[root@localhost /]# useradd task 
[root@localhost /]# useradd taskk
[root@localhost /]# useradd taskkk
**	Show username  : **
[root@localhost /]# tail /etc/passwd
user3:x:1012:1012::/home/user3:/bin/bash
uuv:x:1013:1013::/home/uuv:/bin/bash
ravan:x:1014:1014::/home/ravan:/bin/bash
ram:x:1015:1015::/home/ram:/bin/bash
toop:x:1016:1016::/home/toop:/bin/bash
toopp:x:1017:1017::/home/toopp:/bin/bash
gujju:x:1018:1018::/home/gujju:/bin/bash
task:x:1019:1019::/home/task:/bin/bash
taskk:x:1020:1020::/home/taskk:/bin/bash
taskkk:x:1021:1021::/home/taskkk:/bin/bash
	**Set Userpassword  :**
[root@localhost /]# passwd task
Changing password for user task.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@localhost /]# passwd taskk
Changing password for user taskk.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@localhost /]# passwd taskkk
Changing password for user taskkk.
New password: 
BAD PASSWORD: The password is shorter than 8 characters
Retype new password: 
passwd: all authentication tokens updated successfully.

**	Add groupname  : **
[root@localhost /]# groupadd lecture
	Add users into group  : 
[root@localhost /]# gpasswd -M task,taskk,taskkk lecture
[root@localhost /]# tail /etc/group
uuv:x:1013:toopp
ravan:x:1014:
ram:x:1015:
toop:x:1016:
toopp:x:1017:
gujju:x:1018:
task:x:1019:
taskk:x:1020:
taskkk:x:1021:
lecture:x:12030:task,taskk,taskkk
	**Create directory  :**
[root@localhost /]# mkdir /class
[root@localhost /]# ls
45    boot   direct  etc     ff1   ggggg  jay1  kamlesh  media   opt   sbin    tops  uv.sh   vijay4
afs   class  diz     f1      fid   h1     jay2  l        meet    proc  shukla  tt    var     vijay5
anni  dev    dizz    f11     fit   helo   jay3  leo      mnt     root  srv     tt1   vijay1
asd   di     dizz1   f1.txt  flow  home   jay4  lib      mydir   rr    sys     ttt   vijay2
bin   dii    dizz2   fall    g1    hrdir  jay5  lib64    mydir1  run   tmp     usr   vijay3**
	**Change directory permission for access only users that add into group (lecture)  :****
[root@localhost /]# chmod 770 /class
[root@localhost /]# ls -ld /class
drwxrwx---. 2 root root 6 Jun  1 12:56 /class
[root@localhost /]# chgrp lecture class
[root@localhost /]# ls -ld class
drwxrwx---. 2 root lecture 6 Jun  1 12:56 class

**	**Check this task live  :    log into user and access the directory and create folder and file :
****
 

**	I check to log into another user and try to access directory but it don’t  access because of i create permission of directory 770 , so another user not access the directory.**

 
