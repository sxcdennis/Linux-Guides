# Introduction to RAID, Concepts of RAID and RAID Levels

RAID is a Redundant Array of Inexpensive disks, but nowadays it is called Redundant Array of Independent drives. In a sense, RAID is just a pool of disks (hdd, ssd, nvme etc..) that can become a logical volume.

A RAID contains **groups**, **sets**, or **arrays**. A combinations of 2 or more drives makes a group, set or array.
Only one RAID level can be applied in a group of drives. RAIDs are used for fault tolerance and high availability. A RAID controller is a hardware device used to manage your RAIDs .

RAIDs are managed typically using **mdadm** package in most Linux distributions

This guide will be split into 9 different parts (This being part 1):

**Part 1:** Introduction to RAID, Concepts of RAID and RAID Levels/Types

**Part 2:** RAID0 : Installation and Configuration  (Striping)

**Part 3:** RAID1 : Installation and Configuration  (Mirror)

**Part 4:** RAID5 : Installation and Configuration  (Single Distributed Parity)

**Part 5** RAID6 : Installation and Configuration  ( Double Distributed Parity)

**Part 6:** RAID10 : Installation and Configuration (Mirror & Stripe)

**Part 7:** Growing an Existing RAID Array and Removing Failed Disks in RAID

**Part 8:** How to Recover Data and Rebuild Failed Software RAID’s

**Part 9:** How to Manage Software RAID’s in Linux with ‘Mdadm’ Tool

# Hardware RAID and Software RAID

**Hardware RAID**

Hardware RAIDs are RAID Controllers which is typically built with PCI-e cards. They use NVRAM for caching read and write so if there's a power-failure it will still have backup.

**Software RAID**

Software raid has low performance because it consumes resources from hosts. There is no investment needed for this.


# Concepts of RAID

**Stripe**

Striping is sharing data randomly to multiple disks. This method wont have full data in a single disk. If we use 3 disks half of our data will be in each disk.  

**Mirroring**

Mirroring is making a copy of the same data.

**Parity**

Parity is a method to regenerate lost content from saved information. Basically what it does it send a single data bit at end of each data block.

**Hot Spare**

A spare drive in server that can automatically replace the failed drive.

**Chunks**
Size of data that can be minimum of 4KB and more.


# Common RAID Levels/Types

**RAID0**  Uses Striping  
**RAID1** Uses Mirroring
**RAID5** Uses Singe Distributed Parity
**RAID6** Uses Double Distributed Parity
**RAID10** Combine of Mirror and Stripe



# RAID 0 or Striping

Striping has good performance. In RAID 0 the data will be written to disk using shared method. Half content will be written to one disk and the other half written to the other.

For example:
Lets say we have 2 Disks, and we write data **"DENNIS"** to logical volume. It will be saved as **D** first disk **E** second disk **N** First disk **N** Second disk and continue the process.

In this type of situation, if one of the drives fails we lose all of our data because half the data from one disk can't be rebuilt. But if you want performance RAID0 is great. If you need your data don't use RAID0.

**Quick Overview RAID0:**

- High Performance
- Zero Fault Tolerance (Data will be loss if one breaks)
- Write & Reading quicker

# RAID 1 or Mirroring

RAID1 uses mirroring. Mirroring can have good performance. It essentially makes a copy of the same data. For example we have two 1TB hard drives making a total of 2TB. With RAID0, we can only use/see 1TB because of the mirroring.

If one of the disks fail, we can reproduce data by replacing with a new disk.

**Quick Overview RAID1**
- Good Performance
- Half of total disk usage
- Full Fault Tolerance
- Writing Performance slower
- Reading Performance is fine


# RAID 5 or Distributed Parity

RAID5 is mostly used in enterprise levels. RAID5 has parity distributed across all disks.

Lets say we have 4 drives, if one drive fails we can rebuild the drive by using parity information. Some of the data for parity will be stored for backup in a drive. For example, if we have a 1TB drive you will only be able to use 768GB while 256GB is reserved for backup.

RAID5 can survive only 1 drive failure. If more than 1 drive fails it will cause data loss.

**Quick overview**

- Great performance
- Read will be good speeds
- Writing will be average, slow if no RAID controller  
- Fault Tolerance
- Parity distributed across all drives


# RAID 6 Two Parity Distributed Disk

 RAID 6 is the same as RAID5 but with two parity distributed systems. We need a minimum of 4 Drives.
 RAID6 can survive 2 drive failures at a time.


**Quick Overview RAID6**

- Poor Performance
- Full Fault tolerance
- Parity distributed across all drives
- Can survive 2 drive failures





# RAID 10 or Mirror & Stripe

RAID10 can be called as 1+0 or 0+1. This will be doing both Mirror & Striping. Mirror will be first and stripe will be second in RAID 10.  RAID 10 requires minimum of 4 hard disks.

So in Mirror & Stripe Method we will be writing data to all drives.

RAID10 will use RAID1 in pairs then use RAID0 for the pairs.

Example:

![RAID10](https://github.com/sxcdennis/Linux-Guides/blob/master/images/RAID10.png?raw=true)

**Quick Overview RAID10**

- Good read and write perfroamnce
- Fast rebuild from copying



[< Back: LAMP stack Installation](https://github.com/sxcdennis/Linux-Guides/blob/master/LAMP%20stack%20Installation.md "LAMP stack Installation") || [Next: RAID Part 2 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part2.md "RAID Part 2")
