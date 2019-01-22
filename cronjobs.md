# Cron Jobs

Cron jobs are specific tasks that you can schedule in crobtab. Crontabs is useful for executing jobs automatically in backend.

**crontab** is a command used to manage cron jobs for all users. To manually edit a crontab you can use either crontab -e or edit /etc/crontab

The cron jobs for specific users are located in */var/spool/cron/username*.
You can also schedule your cron jobs by hourly,daily,weekly, and monthly by directing them to:

/etc/cron.hourly
/etc/cron.daily
/etc/cron.weekly
/etc/cron.monthly

## How to edit crontabs

As mentioned earlier, you can access crontabs by *crontab -e* or edit */etc/crontab*

![crontab1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/crontab1.png?raw=true)

**run-parts** will run every script within that directory. In this case it will run
/etc/cron.hourly, /etc/cron.daily ,/etc/cron.weekly,/etc/cron.monthly

As you can see that syntax for each line:

minutes hour day of month month day of week username command

Lets take the fist example:

37 Minutes, Every Hour, Every Day of month , every month, every day of week,  username: root, command run-parts /etc/cron.hourly



## Common crontab Options

**-e**	Edit your crontab file, or create one if it doesn't already exist.

**-l**	Display your crontab file.

**-r**	Remove your crontab file.

**-u user**	Used in conjunction with other options, this option allows you to modify or view the crontab file of user. When available, only administrators can use this option.


## Examples

**1.**  Edit your cron tab

```

crontab -e


```

**2.** Display your crontab


```

crontab -l

```



**3.** Remove your crontab


```

crontab -r


```


**5.** Edit *user1* crontab file

```

crontab -eu user1

```


**6.** List *user1* crontab file


```

crontab -lu user1

```


**7.** Remove *user1* crontab file

```

crontab -ru user1

```


[< Back: Permissions: chmod, chown, chgrp & setfacl](https://github.com/sxcdennis/Linux-Guides/blob/master/chmodacl.md "Permissions chmod & ACL") || [Next: Reset Root Password >](https://github.com/sxcdennis/Linux-Guides/blob/master/resetroot.md "Reset Root Password")
