# Permissions: chmod, chown, chgrp & ACL

On Linux each file and directory is assigned access rights for owner of the file, members of a group and everyone else. Rights/Permissions can be assigned to: **read , write** and **execute**.

Lets take for example we use the **ls -l** command it has the following output:

```

ls -l
-rwxrw-r-- 1 mark mark 0 Dec 28 15:29 file1
-rwxrw-r-- 1 mark mark 0 Dec 28 15:29 file2
-rwxrw-r-- 1 mark mark 0 Dec 28 15:29 file3
-rwXrW-r-- 1 mark mark 0 Dec 28 15:29 file4

```

Now lets dive into what this means.

![file_permissions](https://github.com/sxcdennis/Linux-Guides/blob/master/images/file_permissions.png?raw=true)


Each **-** means it's empty.

The permissions represent: user/owner, group, everyone else for every  3. So for e.g. "rw-rw-r–" shows a file with user permissions "rw-" (means read and write but no execute)and so on.

In the number representation, "r" has value "4", "w" has "2" and "x" has value "1". So the representation for "rw-" is (4+2+0 =) 6 for user. Hence for “-rw-rw-r–” is 664.
Another common one is 755 - rwxr-xr-x. 7 for user 5 for group and 5 for everyone else.

Note: We're ignoring the first bit. Typically you'll see something like -rwxr-xr-x


## ACL

ACLs are a second level of permissions that may override the standard rwx ones. When used correctly they can grant you a better quality in setting access to a file or a directory. For example - by giving or denying access to a specific user that is neither the file owner, nor in the group owner.



In the following we'll talk about 4 different tools:

chmod - modify file access rights
chown - change file ownership
chgrp - change a file's group ownership
getfacl - View acl permissions
setfacl - set acl permissions


# chmod

*chmod* modifies file access rights.



**Syntax**

```

chmod [OPTION]… MODE[,MODE]… FILE…
chmod [OPTION]… –reference=RFILE FILE…

```

**Common Options**

**-c, --changes** like verbose but report only when a change is made

**-v, --verbose** output a diagnostic for every file processed

**--reference source_file destination_file** Replace destination_file permissions with source_file permissions.

**-R, --recursive** change files and directories recursively

**u** user

**g** group

**o** everyone else/others

**a** all

**+** Add permission

**-** Remove permission


**Examples**

1. Make read only file **test.txt** read only by *owner*

```

touch test.txt

chmod 400 test.txt

OR

chmod u-w,g-r,o-r test.txt


```

*4* is the first bit and it represents Read-Only. Then *0* and *0* for the other bits meaning groups and others have no permissions.
You can also do *u-w,g-r,o-r* meaning user - write permissions, group -read permissions and others - read permissions.. As you can see the process with characters is slower than bits.


2. Give all permissions to **test.txt**


```

chmod 777 test.txt

OR

chmod a+rwx test.txt
```

7 gives every permissions, thus doing 777 will give permissions to everyone. The *a* represents all(user,group,others) followed by *+* and *rwx*-read,write,execute.

3. Copy permissions from test.txt to test2.txt

```

touch test2.txt
ls -l
chmod --reference test.txt test2.txt
ls -l

```


# chown

*chown* changes file ownership

**Syntax**

```

chown [OPTION] [OWNER]:[GROUP] File


```

**Common Options**

**-h, --no-dereference** affect each symbolic link instead of any referenced file.

**-f, --silent** suppress most error messages

**-R, --recursive** operate on files and directories recursively

**-v, --verbose** output a diagnostic for every file processed

**Examples**

1. Change owner of *test.txt* to *unknown*

```

chown unknown test.txt


```


2. Change group of *test.txt* to *unknown*

```

chown :unknown test.txt


```
To change the group of a file (chown :group_name file_name)


3. Change both owner and groups back to root

```

chown unknown:unknown test.txt

```


# chgrp

*chgrp* changes a file's group ownership

**Syntax**

```

chgrp [option] groupfile


```

**Common Options**

**-c** like verbose but report only when a change is made

**-R** operate on files and directories recursively

**-v** output a diagnostic for every file processed

**Examples**

1. Change group of *test.txt* to root


```

chgrp root test.txt


```

2. Change group of *test.txt* to unknown verbosely  

```

chgrp -v unknown test.txt

```

3.  Change group of *test.txt* to root with -c option   


```

chgrp -c unknown test.txt

```

# getfacl

*getfacl* views ACL permissions.

**syntax**

```
getfacl [options] file

```

**Common Options**


**-a, --access** Display the file access control list.
**-d, --default** Display the default access control list.


**Examples**

1. Get acl of file.txt

```


getfacl test.txt


```



2.  Display file ACL for file.txt

```

getfacl -a test.txt

````


3. Display default ACL for file.txt

```

getfacl -a test.txt

```


# setfacl

setfacl sets ACL permissions.


**Syntax**

```

setfacl [options] u:user:perms /path/to/file


```

**Common Options**

**-m** modify the ACL of a file or directory. ACL entries for this operation must include permissions.

**-x** remove ACL entries.

**-b, --remove-all** Remove all extended ACL entries. The base ACL entries of the owner, group and others are retained.

**-d, --default** All operations apply to the Default ACL. Regular ACL entries in the input set are promoted to Default ACL entries.

**Examples**

1. Lets say that we have a user named John who doesn't belong to any groups. But we want John to work on certain files. So we give ACL permissions to John (Read and write) to test.txt


```

setfacl -m u:John:rw test.txt


```


2. Now lets say you want to deny access to Marry to test.txt  

```

setfacl -m u:Marry:- test.txt

```

3. Now lets say you want to allow read write access to */home/scripts* to a group named devs.


```

setfacl -m u:devs:rw /home/scripts


```

As you can see setfacl can be quite specific for groups and users.


[< Back: tar](https://github.com/sxcdennis/Linux-Guides/blob/master/tar.md "tar") || [Next: Cron Jobs>](https://github.com/sxcdennis/Linux-Guides/blob/master/cronjobs.md "Cron Jobs")
