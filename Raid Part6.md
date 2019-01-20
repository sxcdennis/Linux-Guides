# RAID10 or 1+0 : Installation and Configuration (Mirror & Stripe)

RAID 10 is a combination of RAID0 and RAID 1. To set up raid 10 we need at least 4 disks.

**My VM Setup**

```

OS: CentOS 7
Disk 1 [20GB]	 :	/dev/sdb
Disk 2 [20GB]	 :	/dev/sdc
Disk 3 [20GB]	 :	/dev/sdd
Disk 4 [20GB]	 :	/dev/sde
```


# Step 1: Installing mdadm and Examine Drives

1. Install mdadm

```
yum -y install mdadm

```

2. Check Drives using mdadm


```

mdadm -E /dev/sd[b-e]

```
# Step 2: Create RAID Partitions

1. Use fdisk on /dev/sdb , /dev/sdc , /dev/sdd , /dev/sde

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

**DO THE SAME STEPS FOR ALL 4 DRIVES!!! (sdb,sdc,sdd,sde)**

2. Check drives using madm -E

```

mdadm -E /dev/sd[b-e]

```

# Step 3: Create md device md0

1. Create md device

```

mdadm --create /dev/md0 --level=10 --raid-devices=4 /dev/sd[b-e]1


```

2. Verify raid level

```

mdadm -E /dev/sd[b-e]1 | egrep -i "level|device"

```

3.   Verify md0

```

mdadm -D /dev/md0

```

# Step 4: Create File System for md0

```

mkfs.ext4 /dev/md0

```


# Step 5: Create mount point and mount md0

```

mkdir /mnt/raid10
mount /dev/md0 /mnt/raid10

```

# Step 6: Create a few files in mountpoint

```

echo raid10 > /mnt/raid10/test.txt
touch /mnt/raid10/hello{1..6}.txt

```

# Step 7: Add entry to fstab & test

1. add entry to fstab

```

/dev/md0  /mnt/raid10   ext4  defaults 0 0

```

2. test using mount -av

```

mount -av

```

# Step 8: Save RAID configuration


```

mdadm -Dvs /dev/md0 >> /etc/mdadm.conf

```

That's pretty much it for RAID10. All of the steps are the same as the last few RAIDs


[< Back: RAID Part 5](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part5.md "RAID Part 5") || [Next: RAID Part 7 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part7.md "RAID Part 7")
