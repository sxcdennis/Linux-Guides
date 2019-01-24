# Filesystems

The primary function of any filesystem is to be a structured place to store and retrieve data.
It also provides rules for naming and structuring data and defines access rights.
**Journaling** keeps track of changes not yet committed to the file system. Meaning if you were to lose power during a file copy it would continue the process instead of being corrupt.

The two most used file systems are **ext4 and xfs** and they both provide journaling.
To create these filesystems we use the command **mkfs**

# Creating ext4  

1. Create partition tables using **fdisk**

```

fdisk /dev/sdd

```

![fs1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/fs1.png?raw=true)


2. Use mkfs.ext4 to create filesystem.

```

mkfs.ext4 /dev/sdd1


```


![fs2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/fs2.png?raw=true)


3. Create mount point

```

mkdir /mnt/drive1


```

4. Use mount

```

mount /dev/sdd1 /mnt/drive1


```

5. Add mount point to **/etc/fstab**

```

/dev/sdd1 /mnt/drive1 ext4  defaults  0 0

```
6. Reload fstab using **mount -a**

```

mount -a

```


# Extending ext4
1. unmount device using **umount**

```

umount /dev/sdd1
df -h

```

2. Use **fdisk** on drive and delete partition table.

![fs4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/fs4.png?raw=true)

3. Re-create table with extended space.

![fs5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/fs5.png?raw=true)


4. Use resize2fs to resize partition.

```

resize2fs /dev/sdd1

```

5. mount the new filesystem using mount

```

mount /dev/sdd1 /mnt/drive1

```

7. reload fs tab using mount -a


```

mount -a

```

# Creating xfs  

1. Create partition tables using **fdisk**


2. Use mkfs.xfs to create filesystem

```

mkfs.xfs /etc/sdd1

```


3. Create mount point

```
mkdir /mnt/drive2

```

4. Use mount to mount filesystem (use mount -o inode64 is fs is above 2TB)

```

mount /dev/sdd1 /mnt/drive2

```



5. Add entry to fstab

```

/dev/sdd1 /mnt/drive2 xfs   defaults  0 0

```

6. Reload fstab using **mount -a**



```

mount -a

```


# Extending xfs  

1. unmount device using **umount**

```
umount /dev/sdd1

```


2. Use **fdisk** on drive to delete partition table




3. Re-create partition table with extended space




4. Use xfs_growfs -d to grow filesystem

```

xfs_growfs -d /dev/sdd1

```

5. Reload fstab using **mount -a**

```

mount -a

```

# Deleteing filesystems

Use wipefs -a to delete or wipe a filesystem.  Make sure to unmount first.


```
wipefs -a /dev/sdd1

```

 [< Back: Partitions](https://github.com/sxcdennis/Linux-Guides/blob/master/partitions.md "Partitions") || [Next: Mounting >](https://github.com/sxcdennis/Linux-Guides/blob/master/mounting.md "Mounting")
