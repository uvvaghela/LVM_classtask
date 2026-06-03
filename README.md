# LVM_classtask

LVM Management
Add a new virtual disk
Create a Physical Volume (PV)
Create a Volume Group (VG)
Create a Logical Volume (LV)
Format it with XFS
Mount it permanently using /etc/fstab

[urvashi@linux9 ~]$ sudo su
[sudo] password for urvashi: 
[root@linux9 urvashi]# cd /

**Perform :**
	**Check Available  disk  :**

[root@linux9 /]# lsblk
NAME          MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sr0            11:0    1 112.5M  0 rom  /run/media/urvashi/CDROM
sr1            11:1    1   9.8G  0 rom  /run/media/urvashi/RHEL-9-3-0-BaseOS-x86_64
nvme0n1       259:0    0    40G  0 disk 
├─nvme0n1p1   259:1    0     1G  0 part /boot
└─nvme0n1p2   259:2    0    39G  0 part 
  ├─rhel-root 253:0    0    37G  0 lvm  /
  └─rhel-swap 253:1    0     2G  0 lvm  [SWAP]
nvme0n2       259:3    0     2G  0 disk 
└─nvme0n2p1   259:4    0     1G  0 part 
  └─myvg-mylv 253:2    0   200M  0 lvm  /lvm
nvme0n3       259:5    0     2G  0 disk 
└─nvme0n3p1   259:6    0   1.5G  0 part 
nvme0n4       259:7    0     2G  0 disk 
nvme0n5       259:8    0     2G  0 disk 
nvme0n6       259:9    0     2G  0 disk 

	**Create PV  :**

[root@linux9 /]# pvcreate /dev/nvme0n4
WARNING: dos signature detected on /dev/nvme0n4 at offset 510. Wipe it? [y/n]: y
  Wiping dos signature on /dev/nvme0n4.
  WARNING: adding device /dev/nvme0n4 with idname eui.f319938568d13da8000c29624cff4e59 which is already used for missing device.
  Physical volume "/dev/nvme0n4" successfully created.
[root@linux9 /]# pvs
  Devices file sys_wwid eui.f319938568d13da8000c29624cff4e59 PVID none last seen on /dev/nvme0n4p1 not found.
  Devices file sys_wwid eui.2e5d58ec41c26037000c2968f650b398 PVID oay02I2mkUBTZi0ToPJEIcPjj9v1A62M last seen on /dev/nvme0n5p1 not found.
  PV             VG   Fmt  Attr PSize    PFree  
  /dev/nvme0n1p2 rhel lvm2 a--   <39.00g      0 
  /dev/nvme0n2p1 myvg lvm2 a--  1020.00m 820.00m
  /dev/nvme0n3p1 myvg lvm2 a--    <1.50g  <1.50g
  /dev/nvme0n4        lvm2 ---     2.00g   2.00g

	**Create VG :**

[root@linux9 /]# vgcreate vgdata /dev/nvme0n4
  WARNING: adding device /dev/nvme0n4 with idname eui.f319938568d13da8000c29624cff4e59 which is already used for missing device.
  Volume group "vgdata" successfully created
[root@linux9 /]# vgs
  Devices file sys_wwid eui.f319938568d13da8000c29624cff4e59 PVID none last seen on /dev/nvme0n4p1 not found.
  Devices file sys_wwid eui.2e5d58ec41c26037000c2968f650b398 PVID oay02I2mkUBTZi0ToPJEIcPjj9v1A62M last seen on /dev/nvme0n5p1 not found.
  VG     #PV #LV #SN Attr   VSize   VFree 
  myvg     2   1   0 wz--n-   2.49g <2.30g
  rhel     1   2   0 wz--n- <39.00g     0 
  vgdata   1   0   0 wz--n-  <2.00g <2.00g

**	Create LV  : **

[root@linux9 /]# lvcreate -L 300M -n mylv vgdata
WARNING: swap signature detected on /dev/vgdata/mylv at offset 4086. Wipe it? [y/n]: y
  Wiping swap signature on /dev/vgdata/mylv.
  Logical volume "mylv" created.
[root@linux9 /]# lvs
  Devices file sys_wwid eui.f319938568d13da8000c29624cff4e59 PVID none last seen on /dev/nvme0n4p1 not found.
  Devices file sys_wwid eui.2e5d58ec41c26037000c2968f650b398 PVID oay02I2mkUBTZi0ToPJEIcPjj9v1A62M last seen on /dev/nvme0n5p1 not found.
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  mylv myvg   -wi-ao---- 200.00m                                                    
  root rhel   -wi-ao---- <36.99g                                                    
  swap rhel   -wi-ao----  <2.01g                                                    
  mylv vgdata -wi-a----- 300.00m             

	**Format Logical Volume with XFS Filesystem  :    **          
                   
[root@linux9 /]# mkfs.xfs -f /dev/vgdata/mylv
meta-data=/dev/vgdata/mylv       isize=512    agcount=4, agsize=19200 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=1, sparse=1, rmapbt=0
         =                       reflink=1    bigtime=1 inobtcount=1 nrext64=0
data     =                       bsize=4096   blocks=76800, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0, ftype=1
log      =internal log           bsize=4096   blocks=16384, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

	**Display UUID Using blkid  : **
 
[root@linux9 /]# blkid /dev/vgdata/mylv
/dev/vgdata/mylv: UUID="d34bb7b3-f29a-48da-a8a1-0ec72f55565f" TYPE="xfs"
[root@linux9 /]# mkdir /tops

**	Add Permanent Mount Entry in /etc/fstab  :**

[root@linux9 /]# vi /etc/fstab

 

	**Verify fstab Configuration Using mount –a  :**

[root@linux9 /]# mount -a
mount: (hint) your fstab has been modified, but systemd still uses
       the old version; use 'systemctl daemon-reload' to reload.

	**Verify Mounted Filesystem Using df –h  :**

[root@linux9 /]# df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 4.0M     0  4.0M   0% /dev
tmpfs                    867M     0  867M   0% /dev/shm
tmpfs                    347M  7.2M  340M   3% /run
/dev/mapper/rhel-root     37G  6.3G   31G  18% /
/dev/nvme0n1p1           960M  299M  662M  32% /boot
/dev/mapper/myvg-mylv    179M   14K  166M   1% /lvm
tmpfs                    174M   92K  174M   1% /run/user/1000
/dev/sr0                 113M  113M     0 100% /run/media/urvashi/CDROM
/dev/sr1                 9.9G  9.9G     0 100% /run/media/urvashi/RHEL-9-3-0-BaseOS-x86_64
/dev/mapper/vgdata-mylv  236M   17M  220M   8% /data


 
