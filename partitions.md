# Partitions

A physical storage device can be divided into multiple logical storage devices known as *partitions*. Each partition is shwon under the */dev/** directory (ex: /dev/sda1 /dev/sda2 /dev/hda2). A partition can be divided into blocks- which are the numbers in after the partitions. You can view blocks by using the command **df -h** or **lsblk**.  

There a 3 types of partitions: Primary , Extended and Swap partitions.

- **Primary partitions** - Are the normal partitions used to install OS, but can be used for others. You can have a maximum of 4 primary partitions.

- **Extended partitions** - Are a solution past primary partitions. A extended partition is divided into many *logical partitions*

- **Swap partitions** Also known as **swap space** is used as a backup when physical memory (RAM) is full so it uses the drive as a resource.


To fully create a proper disk you must have the following:

 - A partition
 - A file System
 - A mount point

 In this section we'll just talk about the partition. We can create partitions using **fdisk**

## Creating Primary Partitions

As mentioned before, Primary partitions can only have a maximum of 4 partitions. To create a primary partition we are going to use **fdisk**


1. First we see what disks are available using lsblk  


```
lsblk

```
![part1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part1.png?raw=true)

As you can see I have 4 disks available sda - sdd. I will use sdb for an example here.

2. Use fdisk on /dev/sdb

```

fdisk /dev/sdb

```
3. Push **n** then use primary partition by pushing **p** then finally choosing a primary partition number 1-4.

![part2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part2.png?raw=true)

4. Choose the size of your block. Choosing nothing will but your block at max size.  Push P to print partition table

![part3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part3.png?raw=true)

5. Push W to save partition table.

6. If you want the kernel to read changes to partition table use **partprobe**


```
partprobe

```
**NOTE:** Please note that you still need a file system and mount point to have this functional.

## Creating Extended Partitions

One of the 4 **primary** partitions need to be replaced by an **extended** partition
have more partitions. Extended partitions can be thought of as containers for **logical partitions**.  A hard disk can only contain **one** extended partition but unlimited amount of **logical partitions** (at least until you've used the complete extended space).

For example: Lets say you've created 1 **primary** partition (sdc1) and 1  **extended** partition (sdc2). Then you start creating logical partitions it will start creating from (sdc5,sdc6 etc..) it can be unlimited.

Lets see this:

1. Use **fdisk** on /dev/sdc

```
fdisk /dev/sdc

```


2. Push **n** then **p** and create 1 primary with **5GB max**

![part4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part4.png?raw=true)


3. Push **n** then **e** and create 1 extended with **10GB max**


![part5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part5.png?raw=true)


4. Push **n** then **l** and create **3** logical partitions each **1GB max**

![part6](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part6.png?raw=true)

5. Push **p** to confirm partition tables.

![part7](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part7.png?raw=true)



6. Push **w** to write partition tables.

**NOTE:** Please note that you still need a file system and mount point to have this functional.


## Creating Swap Spaces

Swap space can take the form of a **partition** or a **file**. The main purpose of a swap space is to extend virtual memory beyond RAM.


### Creating Swap Partition

We'll go over the steps to create a swap partition.

1. Use fdisk on **/dev/sdd**

```

fdisk /dev/sdd

```

2. Push **p** then make the max size **2GB**


![part8](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part8.png?raw=true)


3. Push **t** then **1** then **82**

![part9](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part9.png?raw=true)


4. Have the kernel re-read the tables with **partprobe**

```

partprobe

```

5. Create a swap space for new swap partition **/dev/sdd1**

```

mkswap /dev/sdd1

```
![part10](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part10.png?raw=true)


6. Get **UUID** and add to **/etc/fstab** for mount point(more on mounting later)

```

lsblk -no UUID /dev/sdd1 >> /etc/fstab

```

![part11](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part11.png?raw=true)

7. Edit fstab: find the UUID line and add UUID=IDNUM swap swap defaults 0 0


![part12](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part12.png?raw=true)

8. Use **swapon -a** to activate swap partition

9. Use **swapon -s**  to view swap partitions


10. Use **free -h** to view total memory free


11. Use **lsblk** to veiw blocks.


## Deleting Swap Partition


1. Turn off swap partition using **swapoff /dev/swapspace**

```

swapoff /dev/sdd1

```
![part13](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part13.png?raw=true)

2. Delete or Comment(#) out UUID entry from **/etc/fstab**

![part14](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part14.png?raw=true)

3. Use **fdisk** on disk to delete table. Push **d** to delete.

```

fdisk /dev/sdd

```

![part15](https://github.com/sxcdennis/Linux-Guides/blob/master/images/part15.png?raw=true)

4.  Use partprobe to read new tables.


```

partprobe

```

### Creating Swap File

Swap files are good if you want to change size of swap memory on a regular basis because it's easier to resize a swap file than a partition.  

**fallocate** and **dd** are tools to create swap files. We'll go over **dd**

1. Use dd to create swap space.

```

dd if=/dev/zero of=/swapfile bs=1024 count=100000


```

This will create a 100M space

2. Set permissions for swapspace  

```

chmod 600 /swapfile

```

3. Set up swap file with **mkswap**


```

mkswap /swapfile

```

4. Activate the swap file:

```

swapon /swapfile


```

5.  Edit fstab to add mountpoint for /swapfile


```

/swapfile swap swap defaults 0 0

```

6. Regenerate mount units so that your system registers the new /etc/fstab configuration:

```

systemctl daemon-reload

```

7. To activate the swap file immediately:


```

swapon /swapfile


```

8. Inspect new swap file with free -h

```

free -h


```

### Remove swap file

To remove a swap file, it must be turned off first and then can be removed:

```

swapoff -a
rm -f /swapfile

```

Finally remove the relevant entry from **/etc/fstab**


[< Back: SELinux Modes](https://github.com/sxcdennis/Linux-Guides/blob/master/selinux.md "SELinux Modes") || [Next: Filesystems >](https://github.com/sxcdennis/Linux-Guides/blob/master/filesystems.md "Filesystems")
