# NTP -Configure NTP Server

# Install the NTP package:

`yum install -y ntp`

**1.** Edit the **/etc/ntp.conf** file and comment out the following lines:

```
#server 0.centos.pool.ntp.org
#server 1.centos.pool.ntp.org
#server 2.centos.pool.ntp.org
#server 3.centos.pool.ntp.org

```

**2.** Add own custom public pools.

```

server 0.us.pool.ntp.org
server 1.us.pool.ntp.org
server 2.us.pool.ntp.org
server 3.us.pool.ntp.org

```

**3.** Add a rule to the firewall to allow NTP clients to connect


```

firewall-cmd --permanent --add-service=ntp
firewall-cmd --reload

```

**4.** Activate ntp

```
systemctl enable ntpd
systemctl start ntpd

```


**5.** Check that the NTP service is working properly:

`ntpq -p`


![ntp1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/ntp1.png?raw=true)
