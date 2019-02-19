# firewall basics

firewall can get quite complex. I will just go over the basics of it. The firewall for linux is a daemon called firewalld. Make sure it is install and started. The tool to manage firewall is  **firewall-cmd**

There are other firewalls such as iptables and ufw.

# Firewalld

## Common Options

 **--get-active-zones :** Lists Active Zones

 **--get-services :** Lists Active services

 **--zone=public --list-all :** Lists all information of current zone

 **--permanent --zone=public --add-port=portnum/tcp :** Adds a permanent listening port number to zone

 **--zone=public --list-ports :** Lists public ports

 **--zone=public --add-service=servicename :** Adds service

**--zone=public --remove-service=servicename :** Removes Service

 **--zone=public --list-services :** Lists Service

**--panic-on :** block any incoming or outgoing connections

 **--panic-off :** turns off panic mode

 **--query-panic :** qurey's panic mode on.


## Examples


1. You want to save some changes permanently, so you need to reload the firewall.

```

firewall-cmd --reload

```

2. Get current zones

```
firewall-cmd --get-zones

```
3. Get current services

```

firewall-cmd --get-services


```


4. Get default zone

```

firewall-cmd --get-default-zone


```

5. Open port 80 to permanent zone

```

firewall-cmd --permanent --zone=public --add-port=80/tcp


```


6. Remove port 80.

```

firewall-cmd --permanent --zone=public --remove-port=80/tcp


```

7. List ports

```

firewall-cmd --zone=public --list-ports

```

8. List services

```

firewall-cmd --zone=public --list-services

```


9. Add service ftp


```

firewall-cmd --zone=public --add-service=ftp

```

10. Remove ftp service

```

firewall-cmd --zone=public --remove-service=ftp

```


11. Turn panic mode on

```

firewall-cmd --panic-on

```

12. query panic mode

```
firewall-cmd --query-panic

```
13. turn off panic mode

```

firewall-cmd --panic-on

```


# iptables

iptables is different than firewalld. In a sense it can be a bit more strict because you can deny traffic.

For the full guide: https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html-single/security_guide/#sect-Security_Guide-Firewalls-Using_IPTables


**syntax**

`iptables -A <chain> -j <target>`

**Examples:**

Allow access to port 80 on the firewall
`iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT`

Allow access to port 443 on firewall (HTTPS)
`iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT`

Sometimes you want to accept source ports like for example SSH.

`iptables -A INPUT -p tcp --dport 22 -j ACCEPT`
`iptables -A OUTPUT -p tcp --sport 22 -j ACCEPT`

These rules allow incoming and outbound access for an individual system, such as a single PC directly connected to the Internet or a firewall/gateway. However, they do not allow nodes behind the firewall/gateway to access these services.  To allow NAT you must FORWARD your direct Network card.

For example:To allow forwarding for the entire LAN (assuming the firewall/gateway is assigned an internal IP address on ens33:

`iptables -A FORWARD -i ens33 -j ACCEPT`
`iptables -A FORWARD -o ens33 -j ACCEPT`


# ufw


ufw (uncomplicated firewall) was made to be an easier version of iptables.


**Installing ufw:**

```
yum install -y ufw
or
apt-get install ufw
```

**starting ufw**

`systemctl start ufw `
`systemctl enable ufw` #if you want to have it start on reset


**Examples**

You can add rules by service or port.


`ufw allow 22`
or
`ufw allow ssh `
or
`ufw allow 22/tcp`

This works for denying ports as well.

`ufw deny 22/tcp`

###  Allowing rules from specific ip

Allowing from an ip address
`ufw allow from 192.168.101.89`


Allowing form a specfic subnet

`ufw allow from 192.168.101.89/24`

allow a specific IP address/port combination
`ufw allow from 192.168.101.89 to any port 22 proto tcp`

### Removing Rules

Simply removing rules you can use the delete command

`ufw delete allow 80`

### List status

`ufw status` will list actions of each port 

[< Back: systemctl](https://github.com/sxcdennis/Linux-Guides/blob/master/systemctl.md "systemctl") || [Next: SELinux Modes >](https://github.com/sxcdennis/Linux-Guides/blob/master/selinux.md "SELinux Modes")
