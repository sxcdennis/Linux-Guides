# Nginx

Nginx is a free powerful opened-sourced HTTP webserver. It is often replaced the APACHE, in the LAMP stack therefore calling it LEMP stack. This will be a basic guide on Nginx

## Install  Web Server

Install Nginx HTTP server from the EPEL repository.

```

yum -y install epel-release
yum -y install nginx

```

![nginx1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/nginx1.png?raw=true)

# Enable HTTP Server

Once web server is installed, you must use systemctl to enable it.

```

systemctl start nginx
systemctl enable nginx
systemctl status nginx

```

![nginx2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/nginx2.png?raw=true)

## Configure firewall to allow Nginx Traffic

By default, the built-in firewall is set to block Nginx traffic.
To allow web traffic on Nginx, we must add firewall services for HTTP and HTTPS.

```

firewall-cmd --zone=public --permanent --add-service=http
firewall-cmd --zone=public --permanent --add-service=https
firewall-cmd --reload

```


![nginx3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/nginx3.png?raw=true)


## Test Server

Now you can verify Nginx server by going to the following URL, a default nginx page will be shown.

```

http://YOUR_SERVER_NAME

OR

http://your_sever_ip


```

![nginx4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/nginx4.png?raw=true)



## Important Files and Directories

The default server root directory (top level directory containing configuration files): **/etc/nginx**

The main Nginx configuration file: **/etc/nginx/nginx.conf**

Server block (virtual hosts) configurations can be added in: **/etc/nginx/conf.d**

The default server document root directory (contains web files): **/usr/share/nginx/html**

[<Back: RAID Part 9](https://github.com/sxcdennis/Linux-Guides/blob/master/Raid%20Part9.md "RAID Part 9") || [Next: Mutt >](https://github.com/sxcdennis/Linux-Guides/blob/master/mutt.md "mutt")
