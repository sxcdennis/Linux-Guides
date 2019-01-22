# Permissions: chmod, chown, chgrp & setfacl

On Linux each file and directory is assisnged access rights for owner of the file, members of a group and everyone else. Rights/Permissions can be assigned to: **read , write** and **execute**.

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


## ACL

ACLs are a second level of permissions that may override the standard rwx ones. When used correctly they can grant you a better quality in setting access to a file or a directory. For example - by giving or denying access to a specific user that is neither the file owner, nor in the group owner.



In the following we'll talk about 4 different tools:

chmod - modify file access rights
chown - change file ownership
chgrp - change a file's group ownership
getfacl - View acl permissions
setfacl - set acl permissions


#chmod




#chown




#chgrp




#getfacl




#setfacl







[< Back: tar](https://github.com/sxcdennis/Linux-Guides/blob/master/tar.md "tar") || [Next: Cron Jobs>](https://github.com/sxcdennis/Linux-Guides/blob/master/cronjobs.md "Cron Jobs")
