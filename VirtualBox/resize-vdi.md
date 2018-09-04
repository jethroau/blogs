
## resize vdi to 12GB (dynamic vdi hd)
```
set /a 1024*12
VBoxManage modifymedium --resize 12288 CentOS-7.vdi
```

## extend vdi new space 
```
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


## ref
https://blog.csdn.net/tp7309/article/details/62473010
https://cnzhx.net/blog/resizing-lvm-centos-virtualbox-guest/
https://www.cnblogs.com/pinganzi/p/6478255.html
