<p align="center"> <font size="20"><b> Configuring & Installing KVM on CentOS 7.0  </b> </p></font>
# Video Guide:

**WIP**

# Text Guide:

### **Step 1. If you are in a VM Enviroment make sure you are VM is able to use virtualization: **
For VMWare, under your VM settings make sure **"Virualize Intel VT-x/EPT or AMD-V/RVI"** is checked.


### **Step 2. Install all necessary tools:**
	#yum install -y qemu-kvm libvirt libvirt-python libguestfs-tools virt-install wget

### **Step 3. Download Image file from one of CentOS mirrors available at :**
http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1810.iso

### **Step 4. Enable, Start, and check libvirtd services:**

	# systemctl enable libvirtd
	# systemctl start libvirtd
	# systemctl status libvirtd

### **Step 5. Make sure bridge interface is working**

### **Step 6. Enable IPv4 forwarding:**

### **Step 6a. Edit /usr/lib/sysctl.d/60-libvirtd.conf:**
add line:  net.ipv4.ip_forward = "1"

### **Step 6b. Enable Changes:**
	#/sbin/sysctl -p /usr/lib/sysctl.d/60-libvirtd.conf

### **Step 7. Update the Network Configuration DHCP range for vibr0 interface**
virsh net-edit default
Example:

    <network>
    <name> default</name>
    <uuid>5309509252390-7d4c-4b8f-96-a4232490</uuid>
    <forward mode='nat' />
    <ip address='192.168.1.1' netmask='255.255.255.0' > (change ip address to your default)
    <dhcp>
    <range start='192.168.1.1' end='192.168.1.254' /> (Cahnge this range to yours)
    </dhcp>
    </ip>
    </network>


### **Step 8. Add network routing rule for Guests VM connectivity:**
	#vi /etc/sysconfig/network-scripts/route-virbr0
**Example:**
192.168.101.0/24 via 192.168.101.240 dev vibr0

### **Step 9. Restart**
	#reboot

### **Step 10. Create VM**
	# virt-install --network bridge:virbr0 --name testvm1 --os-variant=centos7.0 --ram=1024 --vcpus=1 --disk path=/etc/kvmtest/images/testvm1.img,size=4 --graphics none --location=/tmp/centosimg/CentOS-7-x86_64-Minimal-1810.iso"

### **Step 11. Install VM through prompts:**






# **KVM Management Tasks**

### **List all VMs on a host:**
	# virsh list --available

### **Display VM information/configuration:**
	#virsh dominfo vmname

### **Stop VM:**
	#virsh shutdown vmname

### **Start VM:**
	#virsh start vmname

### **Enable Autostart of VM:**
	#virsh autostart vmname

### **Delete Gust VM:**
	#virsh shutdown vmname
	#virsh undefine vmname
	#virsh destroy vmname

### **Suspend VM:**
	#virsh suspend vmname

### **Resume VM:**
	#virsh resume vmname

### **Cloning VM :**
	#virsh suspend sourceVM (you can shutdown souce vm too)
	#virt-clone --connect qemu:///system --original sourceVM --name cloneVMname

### **Access VM console:**
	#virsh start vmname (if vm hasn't started)
	#virsh console vmname

### **Go back to host console without shutting down VM:**
To exit a virsh console session without shutting down the VM: **CTRL+ Shift + ]**
