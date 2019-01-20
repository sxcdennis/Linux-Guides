# Growing an Existing RAID Array and Removing Failed Disks in RAID

In this section we'll learn how to grow/add arrays and remove disks from arrays.

# Growing/Adding Array to RAID

**Requirements**

- Need an existing set of RAID Arrays.
- Extra disk to grow away

**Features of RAID Growth**

- Can extend the size of any RAID set
- Can remove faulty disk after growth of an array
- Can grow array array without any downtime.


**My VM Setup**

```

OS: CentOS 7
2 Existing Disks : 20GB
1 Additional Disk: 20GB

```

1. Check existing Array

```

mdadm -D /dev/md0

```

![add1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/add1.png?raw=true)


2. Partition new disk (the same how you did other RAIDS)

```

fdisk /dev/sdd

```


3. Examine new disk

```

mdadm -E /dev/sdd1

```

![add2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/add2.png?raw=true)


4. **Add** new partition to /dev/sdd1 into exisitng array md0

```

mdadm --manage /dev/md0 --add /dev/sdd1

```

![add3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/add3.png?raw=true)


5. Once new disk is added, check if the disk was added in array.

```

mdadm -D /dev/md0

```

![add4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/add4.png?raw=true)

**Note** You can see the drive has been added but only 2 drives are active. In order for us to have 3 drives active, we need to grow the RAID


6. Grow the array

```

mdadm --grow --raid-devices=3 /dev/md0

```
![add5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/add5.png?raw=true)

7. Check new array

```

mdadm -D /dev/md0


```

![add6](https://github.com/sxcdennis/Linux-Guides/blob/master/images/add6.png?raw=true)

**Note**: It may take some time to rebuild but just let it finish. Once it finishes you should have the same output


# Removing Disks from Array

1. Before removing disks we have to mark the disk as failed.

```

mdadm --fail /dev/md0 /dev/sdc1
mdadm -D /dev/md0

```

![rm1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/rm1.png?raw=true)

From that output we can see disk has been marked as faulty.


2. Now we can remove the disk

```

mdadm --remove /dev/md0 /dev/sdc1

```

3. Once faulty drive has been removed, we can grow the raid back to 2 disks.


```

mdadm --grow --raid-devices=2 /dev/md0
mdadm -D /dev/md0


```
![rm2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/rm2.png?raw=true)

Now you can see that there's only 2 drives. And that concludes us removing drives. 



[< Back: RAID Part 6](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part6.md "RAID Part 6") || [Next: RAID Part 8 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part8.md "RAID Part 8")
