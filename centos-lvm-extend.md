# Extend Logical Volume Manager (LVM) partition.

```
lsblk
#sdc                 8:32   0   10G  0 disk
#└─sdc1              8:33   0    5G  0 part
#  └─vg.data-test1 253:0    0   10G  0 lvm  /mnt/test1

# so it is a lvm partition

df -f /mnt/test1

# check volume group
vgdisplay # list volume group

vgdisplay vg.data 

fdisk -l

#
fdisk /dev/sdc
- p # show partition table
- n # select primary, next integer number ...
- t # change id to 8e
- w # save

# need reboot for new partition to take effect

pvcreate /dev/sdc2
pvdisplay # verify

vgextend /dev/vg.data /dev/sdc2

lvextend -l +100%FREE /dev/mapper/vg.data-test1

xfs_growfs /dev/mapper/vg.data-test1

```



