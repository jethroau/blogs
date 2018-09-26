
## resize vdi to 12GB (dynamic vdi hd)
```
set /a 1024*12
VBoxManage modifymedium --resize 12288 CentOS-7.vdi
```

## extend vdi new space 
```
fdisk -l /dev/sda  #check existing partition

fdisk /dev/sda
  n {new partition}
  p {primary partition}
  3 {partition number}

  t {change partition id}
  3 {partition number}
  8e {Linux LVM partition}
  w

fdisk -l /dev/sda

reboot

vgdisplay
# copy VG Name

pvcreate /dev/sda3
vgextend #VG Name /dev/sda3

lvscan
# copy root path

lvextend #root path /dev/sda3

xfs_growfs /dev/centos/root
df -h

lvscan
```

## Example 
```

[root@docker-ce ~]# vgdisplay
VG Name               centos
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  3
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               2
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <7.00 GiB
  PE Size               4.00 MiB
  Total PE              1791
  Alloc PE / Size       1791 / <7.00 GiB
  Free  PE / Size       0 / 0
  VG UUID               07oen5-8Ntg-XcSA-sRQY-Fzly-NbZY-NIOKic

[root@docker-ce ~]# pvcreate /dev/sda3
  Physical volume "/dev/sda3" successfully created.

[root@docker-ce ~]# vgextend centos /dev/sda3
  Volume group "centos" successfully extended
 
[root@docker-ce ~]# lvscan
  ACTIVE            '/dev/centos/swap' [820.00 MiB] inherit
  ACTIVE            '/dev/centos/root' [<6.20 GiB] inherit

[root@docker-ce ~]# lvextend /dev/centos/root /dev/sda3
  Size of logical volume centos/root changed from <6.20 GiB (1586 extents) to 10.19 GiB (2609 extents).
  Logical volume centos/root successfully resized.
 
[root@docker-ce ~]# xfs_growfs /dev/centos/root
meta-data=/dev/mapper/centos-root isize=512    agcount=4, agsize=406016 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=1624064, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 1624064 to 2671616

```

## ref
https://blog.csdn.net/tp7309/article/details/62473010
https://cnzhx.net/blog/resizing-lvm-centos-virtualbox-guest/
https://www.cnblogs.com/pinganzi/p/6478255.html
