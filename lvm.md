# LVM

LVM is a tool for logical volume management which includes allocating disks, striping, mirroring and resizing logical volumes. Before we get into LVM there needs to be terminology defined.

**Physical Volumes**
Prefix: pv
Description: Physical volumes can be defined as physical devices or partitions within the device. For example: A physical device can be **/dev/sdb** **/dev/sdc** or **/dev/sdb1 /dev/sdc1 etc..**

**Volume Group**
Prefix: vg
Description: A combination pool of physical volumes. Example: /dev/sdb + /dev/sdc = Volgroup1 OR /dev/sdb1 + /dev/sdc1 = Volgroup2


**Extents**

Description: Helps allocate Logcial Volumes.


**Logcial Volumes**
Prefix: pv
Description:  A logical/virtual partition that can be is made of multiple extents. This can be formatted with a file system.  

![lvm1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm1.png?raw=true)




**OR**


![lvm2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm2.png?raw=true)



## Steps to creating a Logical Volume

1. **optional** Use **fdisk** to create partition. (8e for LVM type)

2. Create physical volume(s) with **pvcreate**

3. Create volume group with physical volume(s) using **vgcreate**

4. Create Logical Volume(s) with volume group using **lvcreate**

5. **optional** Create a filesystem on logical volume(s)

6. **optional** Create mountpoint and mount filesystem.

### Example of Creating Logical Volume

1. Create partitions on **/dev/sde, /dev/sdf, /dev/sdg**. Have them as the **2nd** partition and all **10G** each (**//dev/sde2, /dev/sdf2, /dev/sdg2**).

![lvm3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm3.png?raw=true)

2. Create Physical volumes for **/dev/sde2 ,dev/sdf2 , /dev/sdg2** using **pvcreate**

```

pvcreate  /dev/sd[e-g]2

```

![lvm4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm4.png?raw=true)

Note: you can display the physical volumes using **pvdisplay** or **pvs**

3. Create volume group named **volumegroup01** for  **/dev/sde2 ,dev/sdf2 , /dev/sdg2** using **vgcreate**

```

vgcreate volumegroup01 /dev/sd[e-g]2

```
![lvm5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm5.png?raw=true)

**Note:** You can view created volume groups by using **vgs** or **vgdisplay**

4. Now we know that we have 3 drives together in a volume group **(volumegroup01)** for a total of around **30G**. Lets create **4 Logical volumes 5G** each and have the names: **lv1home**, **lv1data1**, **lv1data2**, **lv1data3** .

lvcreate -L <size> <volume_group> -n <logical_volume>

```

lvcreate -L 5G volumegroup01  -n lv1home
lvcreate -L 5G volumegroup01  -n lv1data1
lvcreate -L 5G volumegroup01  -n lv1data2
lvcreate -L 5G volumegroup01  -n lv1data3

```

![lvm6](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm6.png?raw=true)


**Note** To display the logical volumes use **lvs** or **lvdisplay**

5. Now we know the new logical volumes are created in **/dev/volumegroup01** lets create **ext4** filesystems for every logical volume.

```

mkfs.ext4 /dev/volumegroup01/lv1home
mkfs.ext4 /dev/volumegroup01/lv1data1
mkfs.ext4 /dev/volumegroup01/lv1data2
mkfs.ext4 /dev/volumegroup01/lv1data3


```

![lvm7](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm7.png?raw=true)

6. Create mount points and edit **/etc/fstab** to have them mounted.

```
mkdir /home/lv1
mkdir /home/lv1/home
mkdir /home/lv1/data1
mkdir /home/lv1/data2
mkdir /home/lv1/data3


```


**/etc/fstab**

```
/dev/volumegroup01/lv1home  /home/lv1/home  ext4  defaults  0 0
/dev/volumegroup01/data1  /home/lv1/data1  ext4  defaults  0 0
/dev/volumegroup01/data2  /home/lv1/data2  ext4  defaults  0 0
/dev/volumegroup01/data3  /home/lv1/data3  ext4  defaults  0 0

```

![lv8](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm8.png?raw=true)

7. Use **mount -a** to mount the new filesystems.  Then view them using **df -h**



```

mount -a
df -h


```

![lvm9](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm9.png?raw=true)


## Extending and Reducing Logical volumes

The traditional way too extend a logical volumes you use the command **lvextend** but there's a process to do this. To reduce (**lvreduce**) a logical volume there's also a process to do it. The better, more efficient way to do this is **lvresize** with the **-L and --resziefs** options.

### Steps to extend logical volume.

1. unmount logical volume
2. Use **fdisk** to create new partition(s)
3. Use **pvcreate** to create new physical volume(s)
4. Extend volume group by adding the newly create physical volume(s) using **vgextend**
5. Use **lvresize** with **--resizefs** option to extend logical volume.
6. mount back logical volume

### Example of Extending Filesystem

1. Unmount Logical Volume **/home/lv1/home**; Also use **lsblk** to see what mount point to unmount on disk you need to adjust.

```
lsblk
umount /home/lv1/home
umount /home/lv1/data3

```


2. Use **fdisk** to create new partition on **/dev/sde** and make it **+3G**

```

fdisk /dev/sde

```

Note: Remember to change type to 8e

3. Create new physical volume  **/dev/sde3** with **pvcreate**  

```

pvcreate /dev/sde3

```

![lvm10](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm10.png?raw=true)


4. Extend volume group by adding **/dev/sde3** to **volumegroup01** using **vgextend**

```

vgextend volumegroup01 /dev/sde3

```
![lvm11](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm11.png?raw=true)

5. Use **lvresize** with option **--resizefs** to extend **/dev/volumegroup01/home**

```

lvresize -L +2G --resizefs volumegroup01/lv1home

```

![lvm12](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm12.png?raw=true)

7. Mount back the logical volume. **/home/lv1/home**


```

mount -a

```


### Steps to reduce logical volume

1. Use **lvresize -L -Xsize --resizefs volumegroup/logicalvol** to reduce  or **lvreduce -L size /lv/dir**
2. Mount the filesystem.


## Example of reducing logical volume



1. Use  **lvresize -L -2G --resizefs volumegroup01/home**  


![lvm13](https://github.com/sxcdennis/Linux-Guides/blob/master/images/lvm13.png?raw=true)


2. Mount the filesystem - **/home/lv1/home**



```

mount -a

```

[< Back: Mounting](https://github.com/sxcdennis/Linux-Guides/blob/master/mounting.md "Mounting") || [Next: Creating and Updating yum repositories (including updating kernels) >](https://github.com/sxcdennis/Linux-Guides/blob/master/yum.md "Creating and Updating yum repositories including updating kernels")
