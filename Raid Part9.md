# mdadm tool usage

This part will go over some common options of mdadm.

# Managing RAID Devices with mdadm

Basic Syntax:

```

mdadm --manage RAID options device


```

**Example 1: Adding a device to RAID Array**

```

mdadm --manage /dev/md0 --add /dev/sdd1

```

**Example 2: Markings RAID device as faulty and removing it from the array**


**Step 1: Mark as faulty**

mdadm --manage /dev/md0 --fail /dev/sdb1

**Step 2: Remove from Array**

```

 mdadm --manage /dev/md0 --remove /dev/sdb1

```


**Example 3: Re-adding a device that was part of array which was removed**


1. If we try to re-add /dev/sdb1 to /dev/md0 right now we get an error.

```
mdadm --manage /dev/md0 --re-add /dev/sdb1

mdadm: --re-add for /dev/sdb1 to /dev/md0 is not possible

```

This is because we already have max amount of possible drives. So we have the choice of removing **/dev/sdd1** from the array then adding **/dev/sdb1** or add **dev/sdb1** as a spare.

2. Removing **/dev/sdd1** then adding **/dev/sdb1**


```

mdadm --stop /dev/md0
mdadm --asemble /dev/md0 /dev/sd[b-c]1

```

This should start rebuilding the data.

**Example 4: Replce RAID device with specific disk.**

```

mdadm --manage /dev/md0 --replace /dev/sdb1 --with /dev/sdd1

```

**Example 5: Marking RAID as Read-Only or Read-Write.**

1. Mark Read Only

```

umount /mnt/raid1
mdadm --manage /dev/md0 --readonly
mount /mnt/raid1
touch /mnt/raid1/test1 

```

The above should give an error stating that it's Read only.

2. Mark Read Write

```

umount /mnt/raid1
mdadm  --manage /dev/md0 --stop
mdadm --assemble /dev/md0 /dev/sd[c-d]1
mdadm --manage /dev/md0 --readwrite
mount /mnt/raid1
tocuh /mnt/raid1/test1
ls -l /mnt/radi1/test1

```

As you can see from the above example the permissions are read write.


[< Back: RAID Part 8](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part8.md "RAID Part 8") ||  [Next: Nginx >](https://github.com/sxcdennis/Linux-Guides/blob/master/nginx.md "nginx")
