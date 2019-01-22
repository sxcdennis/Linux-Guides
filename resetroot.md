# Resetting Root Password

Sometimes people lose root password. It is pretty simple to take back control of root.
The following are some steps to take root back.


1. On Kernel Selection Push e  

![root1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/root1.png?raw=true)


2. Find line that says linux16 and add

```
rd.break enforcing=0
```

![root2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/root2.png?raw=true)

![root3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/root3.png?raw=true)


2. After finished adding Push ctl + x

3. Use following commands:

```

mount -o remount,rw /sysroot

chroot /sysroot


```

![root4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/root4.png?raw=true)


4. Change password using

```

passwd root


```

5. Exit shell


```

exit
exit

```

6. login


7. Use following command

```

restorecon /etc/shadow

```

![root5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/root5.png?raw=true)


8. Reboot system

```

reboot

```


9. Login with new password


[<Back: Cron Jobs](https://github.com/sxcdennis/Linux-Guides/blob/master/cronjobs.md "Cron Jobs") || [Next: systemctl >](https://github.com/sxcdennis/Linux-Guides/blob/master/systemctl.md "systemctl")
