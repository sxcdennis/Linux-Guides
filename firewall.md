# firewall basics

firewall can get quite complex. I will just go over the basics of it. The firewall for linux is a daemon called firewalld. Make sure it is install and started. The tool to manage firewall is  **firewall-cmd**

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

[< Back: systemctl](https://github.com/sxcdennis/Linux-Guides/blob/master/systemctl.md "systemctl") || [Next: SELinux Modes >](https://github.com/sxcdennis/Linux-Guides/blob/master/selinux.md "SELinux Modes")
