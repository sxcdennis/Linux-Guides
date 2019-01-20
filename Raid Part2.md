#  RAID0 : Installation and Configuration  (Striping)


# What is RAID0 (Striping)

Striping is the process of dividing data across drives. So suppose we have two disks. What RAID0 will do is, save data split between both disks. This allows High Performance but risk of data loss.

# Requirements

Minimum amount of disks are 2.
Must even numbers disks (example: 2, 4, 6, 8)


**My VM Setup**

```
Operating System: CentOS 7
Operating System:
Two Disks: 40 GB each

```

# Step 1 : Install mdadm

```

yum install mdadm -y


```

# Step 2: Verify Attached Drives

1. Before Creating RAID0, make sure you have two drives that are detected.

```

lsblk

```

![lsblk1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lsblk1.png?raw=true)


2. Check if any of the drives already have any existing raids.

```

mdadm --examine /dev/sd*


```

![mdadmexamine](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmexamine.png?raw=true)


# Step 3: Create Partition for RAIDS

1. Now create partitions for both drives. **sdb** and **sdc**

```

fdisk /dev/sdb

```

- Push n *n* for new partition
- Choose *P* for primary partition
- Next select partition number
- Use default values

![fdisk1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/fdisk3.png?raw=true)

Next create a auto RAID partition.

- Push *t* for changing partition types
- Push *fd* for Linux RAID auto.
- Push *p* to print changes
- Use *w* to write changes

![fdisk2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/fdisk4.png?raw=true)

**DO THE SAME STEPS FOR 2nd DRIVE (sdc)**


2. Verify partitions using mdadm

```

mdadm --examine /dev/sd*


```

# Step 4: Creating RAID md Devices

1. Now create md device (Example: **/dev/md0)**

```

mdadm -C /dev/md0 -l raid0 -n 2 /dev/sd[b-c]1

OR

mdadm --create /dev/md0 --level=stripe --raid-devices=2 /dev/sd[b-c]1


```

- **-C** create
- **-l** level
- **-n** No of raid-devices


![mdadmcreate](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmcreate.png?raw=true)


2. Once md device has been created, verify RAID level, Devices, and Array used.

**RAID LEVEL**

```
cat /proc/mdstat

```


**RAID DEVICE**

```

mdadm -E /dev/sd[b-c]1

```


**RAID ARRAY**

```

mdadm --detail /dev/md0

```

![mdadmdetails](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmdetails.png?raw=true)

3. Create File System on RAID Device

# Step 5. Assign RAID devices to filesystem

1. Create file system using ext4 for md0

```

 mkfs.ext4 /dev/md0

```

2. Create mount point and mount md0  

```

mkdir /mnt/raid1
mount /dev/md0 /mnt/raid1/


```

3. Verify mount

```

df -h

```

4. Add some files with content in it inside the mount point **/mnt/raid0** . Then view the file.

```
echo "Hello eveyone" > /mnt/raid1/hello.txt
cat /mnt/raid1/hello.txt

```

5. Time to create entry in /etc/fstab so you have this mounted on reset.

add following entry.

```

/dev/md0    /mnt/raid1    ext4    defaults 0 0

```
6. run mount -av to see if there's any errors.

```

mount -av

```

# Step 6: Save RAID Configurations

1. Finally, save raid configurations to one of the files to keep configuration for future use.

```

mdadm -Esv >> /etc/mdadm.conf
cat /etc/mdadm.conf

```

![mdadmconfig](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmconfig.png?raw=true)


[< Back: RAID Part 1](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part1.md "RAID Part 1") || [Next: RAID Part 3 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part3.md "RAID Part 3")
