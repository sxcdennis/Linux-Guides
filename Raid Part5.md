# RAID6 : Installation and Configuration  (Double Distributed Parity)



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

mdadm -C /dev/md0 -l raid6 -n 4 /dev/sd[b-e]1


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

mkdir /mnt/raid6
mount /dev/md0 /mnt/raid6

```

# Step 6: Create a few files in mountpoint

```

echo raid6 > /mnt/raid6/test.txt
touch /mnt/raid6/hello{1..6}.txt

```

# Step 7: Add entry to fstab & test

1. add entry to fstab

```

/dev/md0  /mnt/raid6   ext4  defaults 0 0

```

2. test using mount -av

```

mount -av

```

# Step 8: Save RAID configuration


```

mdadm -Dvs /dev/md0 >> /etc/mdadm.conf

```

# Step 9: Add spare drive

1. Add a new drive and go through the same steps as **Step 2** for the new drive.


```

fdisk /dev/sdf

```

2. Include the drive to md0

```

mdadm --add /dev/md0 /dev/sdf1
mdadm -D /dev/md0

```

# Step 10: Test failed drive

1. Test failed drive

```

mdadm --manage --fail /dev/md0 /dev/sdd1

```


![mdadmbuild1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmbuild1.png?raw=true)


2. Check details to see if it starts rebuilding

```

mdadm -D /dev/md0


```

![mdadmbuild2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mdadmbuild2.png?raw=true)

Now we can see that the hd is rebuilding!







[< Back: RAID Part 4](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part4.md "RAID Part 4") || [Next: RAID Part 6 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part6.md "RAID Part 6")
