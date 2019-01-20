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

[< Back: RAID Part 2](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part2.md "RAID Part 2") || [Next: RAID Part 4 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part4.md "RAID Part 4")
