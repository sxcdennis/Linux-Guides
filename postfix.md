# Postfix

Postfix is a free and open-sourced mail transfer agent(MTA) that can route and deliver emails.

# Prerequisites

Set up master DNS Record: [Click Here](https://github.com/sxcdennis/Linux-Guides/blob/master/dns.md "Click here")



# Install Procedure

**1.** Install **postfix** and **mailx**

```

yum install -y postfix

```

**2.** Add firewall exceptions

```

firewall-cmd --permanent --add-service=smtp
firewall-cmd --reload

```

**3.** Activate **postfix**

```
systemctl enable postfix && systemctl start postfix

```


**4.** Lets assume your domain server for mail is called **mail.example.com** on the **192.168.101.0/24** network.
Edit the **/etc/postfix/main.cf** file and change the following:

```

myhostname = mail.example.com
mydomain = example.com
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain
mynetworks = 192.168.101.0/24, 127.0.0.0/8


```

**5.** Check configuration

```
postfix check
```

**6.** Set the SELinux allow_postfix_local_write_mail_spool boolean to ‘on‘:

`setsebool -P allow_postfix_local_write_mail_spool on`


**7.** Restart postfix

`systemctl restart postfix`


**8.** Add user **test** on **DNS master**

`adduser test`

**9.**  Send a test mail

`echo "This is a test." | mail -s "Testing" test@example.com `

**10.** Check users mail

```
su - test
mail
```

![postfix1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/postfix1.png?raw=true)


IT WORKS!


 [< Back: mysql](https://github.com/sxcdennis/Linux-Guides/blob/master/mysql.md "mysql") || [Next: PXE Server >](https://github.com/sxcdennis/Linux-Guides/blob/master/pxe.md "pxe sever")
