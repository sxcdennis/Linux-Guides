# RAID5 : Installation and Configuration  ( Single Distributed Parity)

# What is RAID5

In RAID5, data strips across multiple drives with distributed parity. Parity stores information in each disk. If one disk fails, we can rebuild the data by using parity information after replacing the failed disk.

The minimum requirement for RAID5 is 3 hard drives.

**My VM Setup**

```

OS: CentOS 7
Disk 1 [20GB]	 :	/dev/sdb
Disk 2 [20GB]	 :	/dev/sdc
Disk 3 [20GB]	 :	/dev/sdd

```


# Step 1: Installing mdadm and Examine Drives

1. Install mdadm

```
yum -y install mdadm

```

2. Check Drives using mdadm


```

mdadm -E /dev/sd[b-d]

```
![mdadmcheck3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmcheck3.png?raw=true)



# Step 2: Create RAID Partitions

1. Use fdisk on /dev/sdb  /dev/sdc  /dev/sdd

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

**DO THE SAME STEPS FOR ALL 3 DRIVES!!! (sdb,sdc,sdd)**

2. Check drives using madm -E

```

mdadm -E /dev/sd[b-d]

```

![mdadme](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadme.png?raw=true)


# Step 3: Create md device md0

1. Create md device

```

mdadm -C /dev/md0 -l raid5 -n 3 /dev/sd[b-d]1


```

2. Verify raid level

```

mdadm -E /dev/sd[b-d]1 | egrep -i "level|device"

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

mkdir /mnt/raid5
mount /dev/md0 /mnt/raid5

```

# Step 6: Create a few files in mountpoint

```

echo raid5 > /mnt/raid5/test.txt
touch /mnt/raid5/hello{1..5}.txt

```

# Step 7: Add entry to fstab & test

1. add entry to fstab

```

/dev/md0  /mnt/raid5   ext4  defaults 0 0

```

2. test using mount -av

```

mount -av

```

# Step 8: Save RAID configuration


```

mdadm -Dvs /dev/md0 >> /etc/mdadm.conf

```



[< Back: RAID Part 3](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part3.md "RAID Part 3") || [Next: RAID Part 5 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part5.md "RAID Part 5")
