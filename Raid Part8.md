# How to Recover Data and Rebuild Failed Software RAID’s

In this section we'll cover how to monitor, replace and recover RAID.

**Testing Scenario**

In this example I'll be using **RAID1** but it can work in any scenario.

1. RAID1 is already set up.
2. Disks are both 20GB
3. Mail User Agent (MUA) installed - [Mutt](https://github.com/sxcdennis/Linux-Guides/blob/master/mutt.md "mutt")

![test1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/test1.png?raw=true)


# Setting up RAID Monitoring

1. Add lines to /etc/madm.conf

```

MAILADDR user@domain or localhost

```

![test2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/test2.png?raw=true)


2a. There are several ways to monitor using mdadm. This way you can add to crontab to start on reboot

```

@reboot /sbin/mdadm --monitor --scan --oneshot --delay 1800

```
**--delay** is by seconds .

2b. Email every 30 minutes

```

mdadm --monitor --scan --oneshot --delay 1800 --mail=root@localhost


```


3. You need to set up a Mail User Agent to get this to work. If you haven't installed this yet go to : [Mutt](https://github.com/sxcdennis/Linux-Guides/blob/master/mutt.md "mutt")


# Simulating and Replacing a failed RAID Storage Device

1. To simulate an issue use --manage and --set-faulty.

```

mdadm --manage --set-faulty /dev/md0 /dev/sdc1


```

2. This will result in /dev/sdc1 being marked as faulty. We can see this by using **mdadm -D /dev/md0**

![test3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/test3.png?raw=true)

3. Now we can see that we have a mail.

![test4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/test4.png?raw=true)


4. Lets remove the device from software RAID

```

mdadm /dev/md0 --remove /dev/sdc1


```


5. Then we can remove it form the machien and replace it with a spare part (dev/sdd)


```

mdadm --manage /dev/md0 --add /dev/sdd1


```

Now we can see the system will automatically start rebuilding.


5b. We can test this by creating a file in /mnt/raid1/test.txt then removing /dev/sdb1 from array. If we still see /mnt/raid1 accessible then it works!

6. Now we can re-add /dev/sdbc1


```

mdadm --mange /dev/md0 --add /dev/sdc1

```

# Recovering from a redundancy loss

As stated earlier one disk will automatically rebuild data, but what happens if two disks failed.
Lets simulate this scenario by marking /dev/sdb1 and /dev/sdd1 as faulty.

```

umount /mnt/raid1
mdadm --manage --set-faulty /dev/md0 /dev/sd[b,d]1
mdadm --stop /dev/md0


```

Lets try to recover the data from /dev/sdb1 into /dev/sde1. Note that you have to already of had fd partition created for sde.

```

ddrescue -r 2 --force /dev/sdb1 /dev/sde1


```

![test5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/test5.png?raw=true)


Now lets rebuild the array using /dev/sde1 and /dev/sdf1

**Note** in real situation you use the same name drive that you had to recover.

```

mdadm --create /dev/md0 --level=mirror --raid-devices=2 /dev/sd[e-f]1

```

Once you are writing the array you can watch the process by using watch command.


```

watch -n 1 cat /proc/mdstat

```



[< Back: RAID Part 7](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part7.md "RAID Part 7") || [Next: RAID Part 9 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part9.md "RAID Part 9")
