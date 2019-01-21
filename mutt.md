# Installing Mutt

Mutt is a command line Email client.

**How to Install Mutt**

```

yum install mutt

```

**Configuration Files**

- Main Configuration: To make global changes edit **/etc/Muttrc**
- User Configuration File: If you want to change some user specific configuration they are in **~/muttrc** or **~/.mutt/muttrc** files.

**Basic Syntax of mutt command**

```

mutt options recipient

```

**Read Emails with Mutt**

1. By current logged in user

```

mutt

```

2. By specific user


```

mutt -f /var/spool/mail/username


```


**Send email with mutt**


```

mutt -s "Subject" emailname@email.com


```

**Terminal Controls:**

- **t** Change recipient email address
- **c** Change Cc address
- **a** Attach files as attachments
- **q** Quit from the interface
- **y** Send that email

**Add Carbon copy(Cc) and Blind Carbon copy(Bcc)**

```

mutt -s "Subject of mail" -c <email add for CC> -b <email-add for BCC> mail address of recipient

```


```

mutt -s “Test Email” -c emailname@email.com  -b root@your_loalhost john@server1.com

```

**Send Emails with Attachments**

```

mutt  -s "Subject of Mail" -a <path of  attachment file> -c <email address of CC>  mail address of recipient

```
EX:

```
mutt -s "Backup" -a /backups/backup.tar  -c den@centos55server@example.com root@centos55server@example.com

```

**Use of muttrc file**

If you want to change the senders name and email, then we need to create a file in that user's home directory

Add the following lines to a **.muttrc** file.


```

set from = "user@domain.com"
set realname = "Realname of the user"


```



[< Back: Mail: Postfix, Dovecot, Squirrelmail](https://github.com/sxcdennis/Linux-Guides/blob/master/mail.md "mail")
