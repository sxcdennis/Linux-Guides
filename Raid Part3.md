# RAID1 : Installation and Configuration  (Mirror)

RAID Mirroring means an exact clone (or mirror) of the same data writing two drives. A minimum of two disks are required to create RAID1.


**Features of RAID1**
- Good Perforamnce
- 50% of disk will be loss. 1TB= 500GB
- No data loss if one disk fails


**My VM Setup**

```
Operating System: CentOS 7
Disk 1 [20GB] /dev/sbd
Disk 2 [20GB] /dev/sdc

```


# Step 1: Installing mdadm and Examine Drives

1. Install mdadm

```
yum -y install mdadm

```

2. Check Drives using mdadm


```

mdadm -E /dev/sd[b-c]

```

![mdadmcheck](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmcheck.png?raw=true)

# Step 2: Create RAID Partitions

1. Use fdisk on both /dev/sdb and /dev/sdc

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

![mdadmverify](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmverify.png?raw=true)


# Step 3: Create RAID1 Devices

1. Create RAID1 Device called **/dev/md0**

```

mdadm --create /dev/md0 --level=mirror --raid-devices=2 /dev/sd[b-c]1

```

![createraid1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/createraid1.png?raw=true)

2. Check raid devices

```

mdadm --detail /dev/md0

```

#Step 4 Create Filesystem

1. Create filesystem for md0

```

mkfs.ext4 /dev/md0

```

2. Create & Mount newly created filesystem under /mnt/raid1


```

mkdir /mnt/raid1
mount /dev/md0 /mnt/raid1


```

3. Test to see if files can be created 

```

echo "Hello raid stuff" > /mnt/raid1/test1.txt


```

4. Add auto-mount point of RAID1 so when you reboot it will remount under /etc/fstab.

```

/dev/md0    /mnt/raid1    ext4  defaults  0 0

```
5. test for errors mount -av

```

mount -av

```


6. Save raid configuration for backup.

```

mdadm --detail --scan --verbose >> /etc/mdadm.conf

```

# Step 5: Test to see  what happens in data failure

1. Check active devices

```

mdadm -D /dev/md0 | grep -E "Ative|Working"

```

![activeworking](https://github.com/sxcdennis/Linux-Guides/blob/master/images/activeworking.png?raw=true)

Now we can see the there are 2 devices working and active.
Lets see what happens if one disk is turned off.

2. Turn off/remove a device

The point of this is to check if the raid works. If a device is removed or turned off you should still have all the data.

```

mdadm -D /dev/md0 | grep -E "Ative|Working"


```

![activeworking2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/activeworking2.png?raw=true)

```
cat /mnt/raid1/test.txt

```

See that it still works.



[< Back: RAID Part 2](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part2.md "RAID Part 2") || [Next: RAID Part 4 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part4.md "RAID Part 4")
