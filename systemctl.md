# systemctl

systemctl is a tool used to manage system daemons/services. When you type systemctl without any commands, it will list all the services.



**Syntax**

```
systemctl [command] [service]

```

**Common Commands**


**start** - Starts/Activates service(s)

**stop** - Stops/Deactivates service(s)

**reload** - Reloads services configuration

**restart** - Stops then starts service(s)

**status** - Checks status of service

**enable** - Creates a symbolic link and "installs" service. This does **NOT START** the service

**disable** - Disables service

**reenable** - Re-enables a disabled service




## Examples


1. Enable httpd service


```

systemctl enable httpd

```

2. Start httpd service

```

systemctl start httpd


```

3. Check status of httpd service

```

systemctl status httpd

```

4. Reload httpd configuration

```

systemctl reload httpd

```


5. Restart httpd service

```

systemctl restart httpd

```

6. Stop httpd service

```

systemctl stop httpd

```

7. Disable httpd service

```

systemctl disable httpd

```

8. Enable httpd service


```

systemctl enable httpd

```

9. Start httpd service

```

systemctl start httpd


```

10. Check status of httpd service

```

systemctl status httpd

```



[< Back: Reset Root Password](https://github.com/sxcdennis/Linux-Guides/blob/master/resetroot.md "Reset Root Password") || [Next: firewall >](https://github.com/sxcdennis/Linux-Guides/blob/master/firewall.md "firewall")
