# Automated Installations Using PXE Server and Kickstart Files

The simplest way to create a customize Kickstart file that you can use it further for multiple installations is to manually perform an installation of and copy, after installation process finishes, the file named anaconda-ks.cfg, that resides in /root path, to an accessible network location, and specify the initrd boot parameter inst.ks=protocol://path/to/kickstart.file to PXE Menu Configuration File.

# Prerequisites:

- Set up PXE Network Boot Server: [Click Here](https://github.com/sxcdennis/Linux-Guides/blob/master/pxe.md "Clickhere")


## Create and Copy Kickstart File

**1.** Go to PXE machine and copy anaconda file to ftp
Then change permissions to the copied file.

`cp anaconda-ks.cfg  /var/ftp/pub/
chmod 755 /var/ftp/pub/anaconda-ks.cfg`

**2.** After file has been copied you can make some minimal changes to it.

`vi /var/ftp/pub/anaconda-ks.cfg`


Replace **–url** filed with your network installation source location: **Ex:** –url=ftp://192.168.101.55/pub/

Replace **network –bootproto** with dhcp in case you have manually configured network interfaces on installation process.

Example of mine:

![kickstart1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/kickstart1.png?raw=true)


For more advanced feel free to read Red Hats documentation:
[Click Here](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/installation_guide/sect-kickstart-syntax "Clickhere")

**3** Before attempting to use the file for installs, it's best to verify the config file using **ksvalidator**. To do that we must install **pykickstart**

`yum install -y pykickstart
ksvalidator /var/ftp/pub/anaconda-ks.cfg`


If the file is fine it will show no errors or any output.

## Add Kickstart installation to PXE Server Config


 **1.** Edit defualt config file.

`vi /var/lib/tftpboot/pxelinux.cfg/default`

```
label 5
menu label ^5)  Kickstart
kernel centos7/vmlinuz
append initrd=initrd.img inst.ks=ftp://192.168.101.55/pub/anaconda-ks.cfg
```

 **inst.ks= FTP** network location (replace protocol and network location accordingly if you are using other installation methods such as HTTP, HTTPS, NFS or remote Installation Sources and Kickstart files).

## Configure Clients

You may now connect to your PXE Server via network.
You may want to change the BIOS boot option to view network first. Typically the hotkey for BIOS is **F2**, but all depends on your motherboards default button.

![pxe2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/pxe2.png?raw=true)

You then push **F8** and proceed to installation.

![kickstart2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/kickstart2.png?raw=true)


After letting it load you can start the install!

![kickstart3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/kickstart3.png?raw=true)



[< Back: PXE Server](https://github.com/sxcdennis/Linux-Guides/blob/master/pxe.md "pxe sever")
