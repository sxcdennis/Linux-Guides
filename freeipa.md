# FreeIPA
FreeIPA stands for Free Identity Policy Audit. It's a solution for combining 389 Directory Server, Kerberos, NTP, DNS, LDAP and more.

![FreeIPA1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/FreeIPA1.png?raw=true)

There are two main installation procedures. One that assumes you don't have a DNS installed and the other assumes you do have one installed. In this tutorial we're going to install one with a  DNS.


## Installing FreeIPA Server with DNS

**My VM Specs:**

**OS:** CentOS 7
**RAM:** 4GB
**Hard Drive:** 40GB
**Address:** 192.168.101.137/24
**Gateway:** 192.168.101.2
**DNS server 1:** 8.8.8.8
**DNS Server 2:** 8.8.4.4
**Hostname:** sxc.dennis.local


**1.** Install ipa-server and ipa-server-dns

```

yum -y install ipa-server ipa-server-dns

```



**2.** Use network manager to update all the correct IP's and domain names.

```

nmtui

```

![freeipa2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/freeipa2.png?raw=true)


**2b.** Update dbus and reboot **Note:** This seems to be a bug fix for dbus not working while installing for FreeIPA. Typically you get an error that states: ``'/bin/systemctl start certmonger.service' returned non-zero exit status 1``


```

yum update dbus
reboot


```


**3.** Install ipa-server


```

ipa-server-install

```

![freeipa3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/freeipa3.png?raw=true)

![freeipa4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/freeipa4.png?raw=true)

![freeipa5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/freeipa5.png?raw=true)


**4.** Add ports & service to firewall

```

firewall-cmd --zone=public --permanent --add-port={80,443,389,636,88,46,53}/tcp
firewall-cmd --zone=public --permanent --add-port={88,123,53}/udp
firewall-cmd --permanent --add-service=freeipa-ldap
firewall-cmd --reload

```

## FreeIPA Commands

**Check Status**

```
ipactl status

```

![freeipa6](https://github.com/sxcdennis/Linux-Guides/blob/master/images/freeipa6.png?raw=true)


**Add user**

```

kinit admin
ipa user-add
ipa passwd sysadmin


```

![freeipa7](https://github.com/sxcdennis/Linux-Guides/blob/master/images/freeipa7.png?raw=true)


**Useful commands:**

ipa user-add     Create new user.
ip user-del     Delete user.
ipa user-find    Search for users.
ipa user-lock    Lock user account.
ipa user-mod     Modify user.
ipa user-show    Display user.
ipa user-unlock  Unlock user account.
ipa passwd user Changes user password


## Installing Free IPA on Client

**My VM Specs:**

**OS:** CentOS 7
**RAM:** 4GB
**Hard Drive:** 40GB
**Address:** 192.168.101.134/24
**Gateway:** 192.168.101.2
**DNS server 1:** 8.8.8.8
**DNS Server 2:** 8.8.4.4
**Hostname:** sysadmin.dennis.local




![freeipa8](https://github.com/sxcdennis/Linux-Guides/blob/master/images/freeipa8.png?raw=true)


**1.** Install client


```

yum -y install free-ipa-client


```

**2.** Run Installer  


```

ipa-client-install  


```

[< Back: dns](https://github.com/sxcdennis/Linux-Guides/blob/master/dns.md "dns") ||
