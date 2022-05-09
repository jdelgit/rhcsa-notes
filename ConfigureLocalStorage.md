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

Remove partition from volume-group

`vgreduce <group-name> <lvm-partition#>`


---
## Create and delete logical volumes

---
## Configure systems to mount file systems at boot by universally unique ID (UUID) or label

---
## Add new partitions and logical volumes, and swap to a system non-destructively