# Installing DNS

Domain Name System (DNS), translates hostnames or URLs into IP addresses. For example we type in www.google.com and the DNS server translates the domain name into associated IP address. Since IP addresses are hard to remebmer all the time, DNS servers are used to translate hostnames like www.google.com to 173.xxx.xx.xxx, so it's easier to remember instead of an IP address.


This guide will help set up local DNS using **Bind** on CENTOS 7, but this should work for RHEL too. There are other software such as **Unbound**, **Erl-DNS**, and **Dnsmasq**. 

# DNS Server Install

**My VM Setup**

I'm going to have 3 nodes. One as Master DNS server, second as Secondary DNS and third will be our client.

Here's my details:


**Master DNS Server Details:**

```
OS:   CentOS7
Hostname:   masterdns.dennis.local
IP: 192.168.101.132/24


```



**Secondary(Slave) DNS Server Details:**


```

OS:   CentOS7
Hostname: secondarydns.dennis.local
IP: 192.168.101.136/24

```

**Client Details:**

```

OS:   CentOS7
Hostname: client.dennis.local
IP: 192.168.101.137/24

````  


## Setting up Master/Primary


1. Install bind packages on server.

```

yum -y install bind bind-utils


```


2. Configure DNS Server by editing **/etc/named.conf**

Add lines marked with # on end
And # for v6
Also add allow-transfer line.

```

options {
    listen-on port 53 { 127.0.0.1; 192.168.101.132/24;}; ### Master DNS IP ###
#    listen-on-v6 port 53 { ::1; };
    directory     "/var/named";
    dump-file     "/var/named/data/cache_dump.db";
    statistics-file "/var/named/data/named_stats.txt";
    memstatistics-file "/var/named/data/named_mem_stats.txt";
    allow-query     { localhost; 192.168.1.0/24;}; ### IP Range ###
    allow-transfer { localhost; 192.168.1.102; };   ### Slave DNS IP ###

```

Add to end of file before includes:


```

zone "dennis.local" IN {
type master;
file "forward.dennis";
allow-update { none; };
};
zone "101.168.192.in-addr.arpa" IN {
type master;
file "reverse.dennis";
allow-update { none; };
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";


```

3. Create Zone files

Create zone files we mentioned inside previous file

3.1 Create Forward Zone

Create **forward.dennis** file in the **/var/named** directory.


```

vi /var/named/forward.dennis

```

Then add the following lines to this file


```

$TTL 86400
@   IN  SOA     masterdns.dennis.local. root.dennis.local. (
        2011071001  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
@       IN  NS          masterdns.dennis.local.
@       IN  NS          secondarydns.dennis.local.
@       IN  A           192.168.101.132
@       IN  A           192.168.101.136
@       IN  A           192.168.101.137
masterdns       IN  A   192.168.101.132
secondarydns    IN  A   192.168.101.136
client          IN  A   192.168.101.137


```


3.2 Create Reverse Zone

Create reverse.dennis file in the **/var/named** directory.

vi /var/named/reverse.dennis


```

$TTL 86400
@   IN  SOA     masterdns.unixmen.local. root.unixmen.local. (
        2011071001  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
@       IN  NS          masterdns.unixmen.local.
@       IN  NS          secondarydns.unixmen.local.
@       IN  PTR         unixmen.local.
masterdns       IN  A   192.168.101.132
secondarydns    IN  A   192.168.101.136
client          IN  A   192.168.101.137
101     IN  PTR         masterdns.dennis.local.
102     IN  PTR         secondarydns.dennis.local.
103     IN  PTR         client.dennis.local.

```

4. Start the DNS service

Enable and start DNS service:

```

systemctl enable named
systemctl start named
systemctl status named

```


5. Firewall Configuration

We must allow the DNS service default port 53 through firewall.

```

firewall-cmd --permanent --add-port=53/tcp
firewall-cmd --permanent --add-port=53/udp

```


6. Configuring Permissions, Ownership, and SELinux
Run the following commands one by one:

```
chgrp named -R /var/named
chown -v root:named /etc/named.conf
restorecon -rv /var/named
restorecon /etc/named.conf

```


7. Test DNS configuration and zone files for any syntax errors
Check DNS default configuration file:

```

named-checkconf /etc/named.conf


```

If it returns nothing, your configuration file is valid.



8. Check Forward zone:

```

named-checkzone dennis.local /var/named/forward.dennis

```

Sample output:

```
zone dennis.local/IN: loaded serial 2011071001
OK

```

9. Check reverse zone:

```

named-checkzone dennis.local /var/named/reverse.dennis


```


Sample Output:

```
zone dennis.local/IN: loaded serial 2011071001
OK

```

10. Add the DNS Server details in your network interface config file.

```

vi /etc/sysconfig/network-scripts/ifcfg-ens33


```

Add Line:


```

DNS="192.168.101.132"

```

11. Edit file /etc/resolv.conf,

```

vi /etc/resolv.conf

```
Add the name server ip address:

```
nameserver      192.168.101.132

```

**Note:** This can all be done with nmtui

![dns1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/dns1.png?raw=true)



12. Test DNS Server with dig

```
dig masterdns.dennis.local

```


Sample Output:


![dns2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/dns2.png?raw=true)




13. nslookup

```
nslookup dennis.local

```

![dns3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/dns3.png?raw=true)


# Set Up Slave /Secondary


Install bind packages using the following command:

```

yum install bind bind-utils -y


```


1. Configure Slave DNS Server

Edit file **/etc/named.conf**:

Make changes to IP lines that have  ###


```
options {
listen-on port 53 { 127.0.0.1; 192.168.101.136; }; ### Secondary IP ###
listen-on-v6 port 53 { ::1; };
directory "/var/named";
dump-file "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
allow-query     { localhost; 192.168.101.0/24; }; ### IP Range ###

```


Add to end of file before include


```

zone "dennis.local" IN {
type slave;
file "slaves/dennis.fwd";
masters { 192.168.101.132; };
};
zone "101.168.192.in-addr.arpa" IN {
type slave;
file "slaves/dennis.rev";
masters { 192.168.101.132; };
};
include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";

```

2. Start the DNS Service

```
systemctl enable named
systemctl start named

```


3. Now the forward and reverse zones are automatically replicated from Master DNS server to **/var/named/slaves/** in Secondary DNS server.

```

ls /var/named/slaves/

```

Sample Output:

dennis.fwd  dennis.rev


4. Add the DNS Server details

Add the DNS Server details in your network interface config file.


```

vi /etc/sysconfig/network-scripts/ifcfg-ens33


```

**Add lines:**


```

DNS1="192.168.101.132"
DNS2="192.168.101.136"

```




5. Edit file **/etc/resolv.conf**


```

vi /etc/resolv.conf


```

Add the name server ip address:


```

nameserver      192.168.101.132
nameserver      192.168.101.136

```


6. Restart network service:


```

systemctl restart network


```

**Note:** This can all be done with **nmtui** tool


![dns4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/dns4.png?raw=true)


7. Firewall Configuration

We must allow the DNS service default port 53 through firewall.

```

firewall-cmd --permanent --add-port=53/tcp

```

8. Restart Firewall

```

firewall-cmd --reload

```
9. Configuring Permissions, Ownership, and SELinux

```
chgrp named -R /var/named
chown -v root:named /etc/named.conf
restorecon -rv /var/named
restorecon /etc/named.conf

```
7. Test DNS Server

```

dig masterdns.dennis.local

```


Sample Output:

![dns5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/dns5.png?raw=true)

```

dig secondarydns.unixmen.local

```

Sample Output:


![dns6](https://github.com/sxcdennis/Linux-Guides/blob/master/images/dns6.png?raw=true)



# Client Configuration

Add the DNS server details in **/etc/resolv.conf** file in all client systems


```

# Generated by NetworkManager
search dennis.local
nameserver 192.168.101.132
nameserver 192.168.101.136

```

**Note:** You can also set this up in nmtui


![dns7](https://github.com/sxcdennis/Linux-Guides/blob/master/images/dns7.png?raw=true)



[< Back: Mutt](https://github.com/sxcdennis/Linux-Guides/blob/master/mutt.md "mutt") || [Next: freeipa >](https://github.com/sxcdennis/Linux-Guides/blob/master/freeipa.md "freeipa")
