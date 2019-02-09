# Installing PXE Network Boot Server for Multiple Linux Installations

**PXE Server** – Preboot eXecution Environment – instructs a client computer to boot, run or install an operating system directly form a network interface, eliminating the need to burn a CD/DVD or use a physical medium, or can ease the job of installing Linux distributions on your network infrastructure on multiple machines the same time

### Requirements

- Configure Static IP Address

- Install NTP Server to Set Correct System Time: [Click Here](https://github.com/sxcdennis/Linux-Guides/blob/master/ntp.md "ntp")


## Installation Procedure

To have a fully functional PXE Network Boot Server, you would need to install a few things: **dnsmsaq** , **syslinux**, and **ftp server** (we'll use **tftp**)


## Install DNSMASQ Server

DNSMASQ is a lightweight DNS manager that provides DNS, DHCP, router advertisement and network boot. It can serve the names of local machines which are not in the global DNS. As opposed to **bind** and **unbound** I would say it's a bit more easier to set up and more leightweight.


```

yum install -y dnsmasq

```

**2.** The DNSMASQ main configuration file is located in the **/etc** directory. Most of it is self-explanatory but it's pretty hard to read because everything is commented.

First make sure you back up the file **/etc/dnsmasq.conf** then open it on an editor.

```
cp /etc/dnsmasq.conf /etc/dnsmasq.conf.backup
vi /etc/dnsmasq.conf

```


**3.** Now you can copy and paste the following into the file. An explanation of each line will be explained after.

```
interface=ens33,lo
#bind-interfaces
domain=pxe.example.com
# DHCP range-leases
dhcp-range= ens33,192.168.101.3,192.168.101.253,255.255.255.0,1h
# PXE
dhcp-boot=pxelinux.0,pxeserver,192.168.101.55
# Gateway
dhcp-option=3,192.168.101.2
# DNS
dhcp-option=6,192.168.101.55
server=8.8.8.8
# Broadcast Address
dhcp-option=28,192.168.101.255
# NTP Server
dhcp-option=42,0.0.0.0

pxe-prompt="Press F8 for menu.", 60
pxe-service=x86PC, "Install CentOS 7 from network server 192.168.101.55", pxelinux
enable-tftp
tftp-root=/var/lib/tftpboot

```

You can check the config file by using --test option with dnsmasq

```

dnsmasq --test

```


- **interface** – Interfaces that the server should listen and provide services.

- **bind-interfaces** – Uncomment to bind only on this interface.

- **domain** – Replace it with your domain name.

- **dhcp-range** – Replace it with IP range defined by your network mask on this segment.

- **dhcp-boot** – Replace the IP statement with your interface IP Address.

- **dhcp-option=3,** 192.168.101.2 – Replace the IP Address with your network segment Gateway.

- **dhcp-option=6,** 192.168.101.55 – Replace the IP Address with your DNS Server IP – several DNS IPs can be defined.

- **server=** 8.8.8.8 – Put your DNS forwarders IPs Addresses.

- **dhcp-option=28,** 192.168.101.255 – Replace the IP Address with network broadcast address –optionally.

- **dhcp-option=42,** 0.0.0.0 – Put your network time servers – **optionally** (0.0.0.0 Address is for self-reference).

- **pxe-prompt** – Leave it as default – means to hit F8 key for entering menu **60** with seconds wait time..

- **pxe=service** – Use x86PC for 32-bit/64-bit architectures and enter a menu description prompt under string quotes. Other values types can be: PC98, IA64_EFI, Alpha, Arc_x86, Intel_Lean_Client, IA32_EFI, BC_EFI, Xscale_EFI and X86-64_EFI.

- **enable-tftp** – Enables the build-in TFTP server.

- **tftp-root** – Use /var/lib/tftpboot – the location for all netbooting files.


# Installing Syslinux Bootloaders

**Syslinux** is a liightweight master boot record (MBR) boot loader. Syslinux is a bundle suite that includes ISOLINUX, PXELINUX and EXTLINUX.

**1.** Install syslinux

```

yum install -y syslinux

```

# Install TFTP-Server and Populate it with Syslinux Bootloaders

**TFTP** is a file transfer protocol that uses **UDP**(69) as opposed to **FTP** which uses **TCP**. It is a connectionless protocol and has simpler commands.

**1.** Install tftp and populate its boot files with syslinux files.

`yum install -y tftp-server`
`cp -r /usr/share/syslinux/* /var/lib/tftpboot`


# Set up PXE Server Configuration File

Typically, the PXE Server reads its configuration from a group of specific files (GUID files – first, MAC files – next, Default file – last) hosted in a directory called pxelinux.cfg, which must be located in the directory specified in tftp-root statement from DNSMASQ main configuration file.

**1.** Create the directory **pxelinux.cfg**  

`mkdir /var/lib/tftpboot/pxelinux.cfg `


**2** Create the default file.

`vi /var/lib/tftpboo/pxelinux.cfg/default `

Below you can see an example configuration file that you can use it, but modify the installation images (kernel and initrd files), protocols (FTP, HTTP, HTTPS, NFS) and IPs to reflect your network installation source repositories and paths accordingly.

```
default menu.c32
prompt 0
timeout 300
ONTIMEOUT local

menu title ## PXE Boot Menu ##

label 1
menu label ^1) Install CentOS 7 x64 with Local Repo
kernel centos7/vmlinuz
append initrd=centos7/initrd.img method=ftp://192.168.101.55/pub devfs=nomount

label 2
menu label ^2) Install CentOS 7 x64 with http://mirror.centos.org Repo
kernel centos7/vmlinuz
append initrd=centos7/initrd.img method=http://mirror.centos.org/centos/7/os/x86_64/ devfs=nomount ip=dhcp

label 3
menu label ^3) Install CentOS 7 x64 with Local Repo using VNC
kernel centos7/vmlinuz
append  initrd=centos7/initrd.img method=ftp://192.168.101.55/pub devfs=nomount

label 4
menu label ^4) Boot from local drive

```
As you can see CentOS 7 boot images (kernel and initrd) reside in a directory named **centos7** relative to **/var/lib/tftpboot** (on an absolute system path this would mean **/var/lib/tftpboot/centos7**) and the installer repositories can be reached by using FTP protocol on **192.168.101.55/pub** network location – in this case the repos are hosted locally because the IP address is the same as the PXE server address).


## Add CentOS 7 Boot Images to PXE Server

For this step CentOS **kernel** and **initrd files** are required. To get those files you need the **CentOS 7 DVD ISO Image**. So, go ahead and **download** CentOS **DVD** Image, and mount the image to /mnt system.

**1.** Download dvd and mount it to **/mnt**

```
wget http://mirror.sfo12.us.leaseweb.net/centos/7.6.1810/isos/x86_64/CentOS-7-x86_64-DVD-1810.iso

mount -o loop /path/to/centos-dvd.iso /mnt

```

After the DVD content is made available, create the **centos7 directory** and copy CentOS 7 **bootable kernel** and **initrd images** from the DVD mounted location to centos7 folder structure.


```
mkdir /var/lib/tftpboot/centos7
cp /mnt/images/pxeboot/vmlinuz  /var/lib/tftpboot/centos7
cp /mnt/images/pxeboot/initrd.img  /var/lib/tftpboot/centos7

```
The reason for using this approach is that, later you can create new separate directories in **/var/lib/tftpboot** path and add other Linux distributions to PXE menu without messing up the entire directory structure.

## Create CentOS 7 Local Mirror Installation Source

Although you can setup Installation Source Mirrors via a variety of protocols such as **HTTP, HTTPS or NFS**, for this guide, I have chosen **FTP** protocol because is very reliable and easy to setup with the help of **vsftpd** server.


**1.** Install **vsftpd** daemon, copy all DVD mounted content to vsftpd default server path (**/var/ftp/pub**), and append readable permissions to this path.

```
yum -y install vsftpd
cp -r /mnt/* /var/ftp/pub
chmod -R 755 /var/ftp/pub

```
## Start and enable Daemons for DNSMASQ and VSFTPD

```
systemctl start dnsmasq && systemctl enable dnsmasq
systemctl start vsftpd && systemctl enable vsftpd

```
## Open Firewall and Test FTP Install Source

```
firewall-cmd --permanent --add-service=ftp
firewall-cmd --permanent --add-service=dns   	
firewall-cmd --permanent --add-service=dhcp   	
firewall-cmd --permanent --add-port=69/udp   	
firewall-cmd --permanent --add-port=4011/udp  
firewall-cmd --reload
```

**2.** To test FTP installation, open up a browser and type in your ftp://ip address followed by /pub.

ftp://192.168.101.55/pub

![pxe1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/pxe1.png?raw=true)

**3.** Now you can unmount the dvd

`umount /mnt`


## Configure Clients to Boot form Network

Now your clients can boot and install CentOS7 on their machines by configuring **Network Boot** as a primary boot device from their system **BIOS** (**F2** to enter) or hitting specified key during BIOS POST operation as specified in motherboard manual.


![pxe2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/pxe2.png?raw=true)

In order to choose network booting. After first PXE prompt appears, press **F8** key to enter presentation and then hit Enter key to proceed forward to PXE menu.

![pxe3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/pxe3.png?raw=true)

![pxe4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/pxe4.png?raw=true)

Once you've reached PXE menu, choose installation type and hit Enter to continue. Note that some may require internet connection.

![pxe5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/pxe5.png?raw=true)





[< Back: postfix](https://github.com/sxcdennis/Linux-Guides/blob/master/postfix.md "postfix") || [Next: kickstart >](https://github.com/sxcdennis/Linux-Guides/blob/master/kickstart.md "kickstart")
