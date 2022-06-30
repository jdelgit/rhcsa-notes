## List, create, delete partitions on MBR and GPT disks

MBR : Master Boot Record
GPT : GUID Partition Table

List Disk size, mount-points and available space `df -h`

List disk UUID `blkid`

List block devices `lsblk`

Check for any partition changes `partprobe`

### Create MBT partition

Use `fdisk -l` to list all available disks and partitions

`fdisk <disk>`

Use the help menu to see available commands

Primary (p): Generaly used for bootable partition
Extended(e): Other


Add partition

`Command (m for help):  n`

Choose partition number

`Partition number (1-4, default 1):  `

Choose first sector

`First sector (2048 - 20971519, default 2048):  `

Choose last sector, can set certain size e.g. +5G (+5 Gigabytes)

`Last sector, sector or +size{K,M,G,T,P} (2048 - 20971519, default 2048):   `

List partitions

`Command (m for help):  p`

Set partition type

`Command (m for help):  t`

Write (save) changes

`Command (m for help):  w`

Use `fdisk -l` to confirm changes. Or `lsblk` to view

### Delete partition

Use `fdisk` to access partition editor

`fdisk <disk>`

Delete partition

`Command (m for help):  d`

Write changes

`Command (m for help):  w`

### Create GPT partition

Use `gdisk -l` to list all available disks and partitions

Entir `gdisk` editor for desired disk

`gdisk <disk>`

Add partition

`Command (m for help):  n`

Choose partition number

`Partition number (1-128, default 1):  `

Choose first sector

`First sector (34 - 20971486, default = 2048) or {+-}size{KMGPT}:  `

Choose last sector, can set certain size e.g. +5G (+5 Gigabytes)

`Last sector (2048 - 20971486, default = 20971486) or {+-}size{KMGPT}: `

Choose file system type (default Linux filesystem)

`Hex code or GUID (L to show codes, Enter = 8300): `

Write changes

`Command (m for help):  w`

Confirm to proceed

---
## Create and remove physical volumes

List physical volume 

`pvdisplay`

Create primary partition and set type as *Linux LVM*

Create physical volume

`pvcreate <parition>`

Remove physical volume

`pvremove <partition>`

---
## Assign physical volumes to volume groups

List volume groups

`vgs`

Create multiple primary partitions

Convert the partitions to LVM partition type

Create volume group with partition

`vgcreate <group-name> <lvm-partition1>`

Add second partition

`vgextend <group-name> <lvm-partition2>`

Remove partition from volume group

`vgreduce <group-name> <lvm-partition#>`

Delete volume group

`vgremove <group-name>`


---
## Create and delete logical volumes

`lvs` to display logical volumes

Create logical volume using created volume group

`lvcreate -L <size> -n <logical-volume-name> <volume-group-name>`

List logical volumes with verbose information

`lvdisplay`

Extend logical volume

`lvextend -L<size-to-extend> <logical-volume-path>`

Resize logical volume (potential data loss)

`lvresize -L-<resize> <logical-volume-path>`

Remove logical volume 

First unmount logical volume

`umount <logical-volume-path>`

`lvremove <logical-volume-path>`

Verify removal with `lvs` and `lvdisplay`

---

## Configure systems to mount file systems at boot by universally unique ID (UUID) or label

### Create and mount ext4 filesystem using UUID

`mkfs.ext4 <partition-path>`

Copy UUID displayed after creation

To mount on boot the entry must be added to `/etc/fstab`

Edit `/etc/fstab`

```
....
....
UUID=<disk-uuid> <mount-point> <filesystem-type> <options> <dump> <pass>

```

dump: Used by dump-utility to decide when to make a backup. Options 0 or 1. 1 will signal that a backup needs to be done if the dump-utility installed
pass: Determines in which order the filesystem should be checked. Options 0,1, or 2. 0 if the filesystem should not be checked, 1 is highest priority reserved for root fs, 2 for all other filesystems

Once `/etc/fstab` has been configured use `mount <mount-point>` to mount the disk

`umount <mount-point>` to unmount


### Label disk and mount using label

To label disk use `e2label`

`e2label <disk-volume> <label-name>`

Edit `/etc/fstab`

```
....
....
LABEL=<disk-label> <mount-point> <filesystem-type> <options> <dump> <pass>

```

---
## Add new partitions and logical volumes, and swap to a system non-destructively

Create logical volume
`lvcreate -n <swap-name> -L <size> <volume-group>`

Create swapspace using the volume

`mkswap <volume-group-path>`

Turn  on swap `swapon <volume-group-swappath>`

Check summary of the swap 

`swapon -s`

For persistent swapspace, update the swap entry in the `/etc/fstab` then activate with `swapon -a`

Turn off swap `swapoff <volume-group-swappath>`