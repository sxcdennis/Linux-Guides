# User and Group Management

In this guide we'll cover adding, deleting and modifying users and groups.

# useradd

When running the command **useradd** it will perform the following:

- Adds a new entry to both */etc/passwd* and */etc/shadow*
- Adds new entry to */etc/group* and */etc/gshadow*
- Permissions and ownership are also set to home directory by command.


**Basic syntax**

```

useradd -options username

```


**Common options:**

```

-d /path/to/home/directory: The new user will be created with this specified directory

-e YYYY-MM-DD: Date which user will be disabled.

-f Days: Number of days before password is expired

-g GROUP: Group name or number of user

-G Group1,group2,group^n:  Secondary groups followed by commas or just one.

-s /dir/to/shell: Directory to shell of user

-u UID: A number value added for unquie user ID

```

# groupadd

When running the command groupadd it will add an entry to */etc/group*

**syntax**

```

groupadd [option] groupname

```



**Common Options**

```

-f : This will force success even if the group already exists
-g GID: add group with groupID
-p PASSWORD: Add a password with group


```

# passwd

When using **passwd** it will apply changes to the following:

- /etc/passwd
- /etc/shadow
- //etc/pam.d/passwd

**Syntax**

```

passwd [options] username


```

**Common Options**

```

-a : When used with -S this will show password status for all users. Will not work with -S
-d : Delete as users password (making it empty )
-e: Immediately expire a users password.
-i DAYs: Will expire user password with number replace DAYs
-l: Lock password of named account
-S: Display account status of user
-u: Unlock users password
-w DAYS: warn user amount of days before expired
-x MAX_DAYS: Sets number of MAX_DAYs before password expires

```



## Examples of useradd

1. Create a user named Jenny with the group name of HR and UID of 1200 which will expire 2020-05-20.

```

useradd -u 1200 -g HR -e 2020-05-20


```

2. Create a username of John with the secondary group of IT and UID 1201  


```

useradd -u 1201 -G IT John

```



3. Create username of Jacob with no interactive shell and belongs to group HR.  


```

useradd -s /sbin/nologin -g HR Jacob

```

4. Create user Sarah with password boogers and group admin which has a password which will expire in 2 days.


```

useradd -g admin -f 2 Sarah
passwd Sarah

```


# deluser  

deluser deletes users, groups, backsup files  and removes files/directories.

**Syntax:**

```

deluser [options] user group


```


**Common Options**

```

--group GROUPNAME: Deletes GROUPNAME

--system: Only delete if user/group is a system user/group. This avoids accidentally deleting non-system users/groups. Additionally, if the user does not exist, no error value is returned.

--backup: Backup all files contained in the userhome and the mailspool file to a file named /$user.tar.bz2 or /$user.tar.gz.

--backup-to: Place the backup files not in / but in the directory specified by this parameter. This implicitly sets --backup also.

--remove-home: Remove the home directory of the user and its mailspool. If --backup is specified, the files are deleted after having performed the backup.

--remove-all-files: Remove all files from the system owned by this user. If --backup is specified, the files are deleted after having performed the backup.


```


# delgroup

delgroup deletes groups, backup files and remove groupfiles.

**syntax:**

```

delgroup [options] group  


```

**Common Options**

```

--system: Only delete if user/group is a system user/group. This avoids accidentally deleting non-system users/groups. Additionally, if the user does not exist, no error value is returned.

--backup: Backup all files contained in the userhome and the mailspool file to a file named /$user.tar.bz2 or /$user.tar.gz.

--backup-to: Place the backup files not in / but in the directory specified by this parameter. This implicitly sets --backup also.

--remove-home: Remove the home directory of the user and its mailspool. If --backup is specified, the files are deleted after having performed the backup.

--remove-all-files: Remove all files from the system owned by this user. If --backup is specified, the files are deleted after having performed the backup.


```

# usermod

usermod has the options of modifying users with nearly the same options as useradd.

**Syntax**

```

usermod [options] USERNAME


```

**Common options**


```

-c COMMENT: Add new value of the user's password file comment field. It is normally modified using the chfn utility.

-d /home/directory :  /home/directroy is users new home directory.

-e YYYY-MM-DD: The date on which the user account will be disabled.

-f DAYS: Number of DAYS before password expires

-g GROUP: Replace GROUP with name or GID

-G group1,group2,group^n: List of secondary groups

-l NEW_LOGIN_NAME: Name of user will be changed with NEW_LOGIN_NAME

-L: Locks user name

-m: Moves user's home directory content to new home directory. Only works with option -d

-p PASSWORD: Changes users password

-u UID: Changes UID

-U: Unlocks user


```



[< Back: Table of Contents](https://github.com/sxcdennis/Linux-Guides "table of contents") || [Next: grep and find >](https://github.com/sxcdennis/Linux-Guides/blob/master/grepfind.md "grep and find")
