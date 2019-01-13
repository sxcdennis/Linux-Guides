# LAMP Stack Installation

LAMP stands for Linux Apache MySQL or (MariaDB), PHP
This will be a guide to installing AMP.


# Video

WIP

# Set up your IP using nmtui

First start off by checking your ips using `ip a`  following up by using `nmtui`.
Edit a connection.
Edit the addresses for example: 192.168.101.100/24
Gateway: 192.168.101.1
DNS servers: 8.8.8.8

After you're finished, make sure to activate the connection.

# Installing Apache

**1. After setting up your static IP, install httpd**

```

yum install -y httpd

```

**2. Start httpd**

```

# systemctl start httpd
# systemctl status httpd

```

**3. Give firewall permissions**

```

# firewall-cmd --add-service=http

OR if you want firewall to have it permantely

# firewall-cmd --permanent --add-service=http

```

**4. Restart firewalld**


```
# systemctl restart firewalld

```


**5. Verify that you have Apache installed.**

Open a browser and type in your IP. Example:
192.168.101.251

**6. Set your website to index.**

```

# vi /etc/httpd/conf.d/welcome.conf

```

**Change**:  ``Options -Indexes`` to ``Options +Indexes``

**7. Restart httpd**

```

# systemctl restart httpd

```


# Installing PHP

**1. Install php and extensions**

```

# yum install -y php php-mysql php-pdo php-gd php-mbstring

```


**2. Get full list of PHP info from browser**

```

# echo "<?phpinfo(); ?>" > /var/www/html/phpinfo.php
# systemctl restart httpd

```

View the php file on your browser. **Example** 192.168.101.251/phpinfo.php


# Install MariaDB

**1. Install MariaDB and MariaDB-server**

```
# yum install -y mariadb-server mariadb

```

**2. Start mariadb**

```
# systemctl start mariadb

```

**3. Secure instantiation for mariadb**

```
# mysql_secure_installation

```

**4. Test database**

```
# mysql -u root -p
MariaDB > SHOW VARIABLES;
MariaDB > quit

```
