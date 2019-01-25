# Mounting and unmounting

The commands **mount** and **umount** both mount and unmount a filesystem. They cannot survive reboot, if you want to make permanent changes you must add fields in **/etc/fstab**. This guide will go over the basics of  **mount, umount** and **/etc/fstab**

# mount


**mount** command mounts a storage device or filesystem, making it accessible. This command does not permanently mount a filesystem or device.

**Common Uses**


**mount -o option** There are several other options you can put into a mount. The most common ones are **remount**, **rw** (mount as read write), and **ro** (read only)


**mount -a**   This will mount all points that are mentioned in **/etc/fstab**

**mount /dev/location /path/to/mount**  - This will mount the device location to the mount point.

## Examples of mount

1. Lets say you have a usb drive that you want to mount and you want to mount it to **/media/usb**. You used lsblk and see that it's device /sdd1

```

mkdir /media/usb
mount /dev/sdd1 /media/usb

```

2.  Lets say you have a drive **/dev/sdd1/** and it's currently mounted to **/mnt/dev1/**. It is read-only but you want to change to it read-write so you can change some files.


```

mount -o remount,rw /dev/sdd1 /mnt/dev1

```
Note how this is useful only if a filesystem is already mounted.

3. Now lets say you have a drive **/dev/sdd1/** and it's currently mounted to **/mnt/dev1/**. It is read-write but now you want to change to it read-only so other's can't write on it.

```

mount -o remount,ro /dev/sdd1 /mnt/dev1

```

4. Mount **/dev/sde** to **/mnt/dev2** but have it read-only

```

mount -o ro  /dev/sde /mnt/dev2

```

5. Mount **/dev/sde** to **/mnt/dev2** but have it read-write


```

mount -o rw  /dev/sde /mnt/dev2

```

6. You made some changes to **/etc/fstab**. Now you want to access the mount point.

```

mount -a

```

# umount

umount unmounts filesystem or disk .

**Common Options**

**-f** Forcefully unmounts filesystem or disk.

**-l** Lazy unmount: Detaches the filesystem and cleans up filesystem when it's not busy anymore.



### Examples of umount

1. Lets say you want to unmount **/mnt/dev1** point

```
umount /mnt/dev1

```

2. Lets say you used umount onf /mnt/dev1 and it says umount: /mnt/dev1: device is busy.You want to forcefully unmount it.

```

umount -f /mnt/dev1

```

3. Now lets say you used umount -f /mnt/dev1 and it still doesn't work. You still want to unmount it.

```

umount -l /mnt/dev1

```


# /etc/fstab  

**/etc/fstab** file   can be used to define how disk partitions, various other block devices, or remote filesystems should be mounted into the filesystem.

Line Syntax :

```
device        dir        type        options        dump fsck

```

The device can be the /dev/device or block UUID which could be found using

```

blkid -no UUID /device/path

```

### Examples of /etc/fstab

1. This is an example of how a fstab would look

```
#device        dir        type        options        dump fsck

/dev/mapper/centos-swap swap  swap  defaults  0 0
/swapfile swap  swap  defaults  0 0
/dev/sda1 /mnt/dev1 ext4  defaults  0 0
UUID=5fcca3995-7dd7-4d9b-bc4d-db6ea2594cb5  swap  swap  defaults  0 0

```
Notice the differences between swap partitions, UUID , swapfile, and mount points look.

2. Add **swap partition** for **/dev/sdd1** to **/etc/fstab**

2a. Get the UUID and append to /etc/fstab

```


lsblk -no UUID /dev/sdd1 >> /etc/fstab

```

2b. Find the UUID number on last line and add to it.

```

UUID=5fcca3995-7dd7-4d9b-bc4d-db6ea2594cb5  swap  swap  defaults  0 0


```

Note: Dont forget to add the **UUID=** at the beginning of the line. 



[< Back: Partitions](https://github.com/sxcdennis/Linux-Guides/blob/master/partitions.md "Partitions") || [Next: Creating & Maintaining LVM >](https://github.com/sxcdennis/Linux-Guides/blob/master/lvm.md "Creating & Maintaining LVM")
