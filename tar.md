# tar  

Tar (tape archive) is the most used command to archive/compress multiple files into a single file. It can be further compressed using **gzip** and **bzip2** within the tool.

**Syntax**

```

tar [options] [files/directories]

```

### Common Options

**--delete**: Delete from archive

**-r, --append**: append files to end of an archive

**-t, --list**: list contents of an archive

**--test-label** test the archive volume label and exit

**-u, --update** only append files newer than copy in arhive

**-x,--extract** extract files from an archive

**-C,--directory=DIR** change to directory DIR

**-f, --file=ARCHIVE** use archive file or device ARCHIVE

**-p, --preserve-permissions** preserve security permissions of files  

**-v, --verbose** verbosely list files

**-z,--gzip** further compress archive through gzip

**j,--bzip2** further compress archive to bzip2


# Examples


## Example 1: Create tar

```
tar -cvf scripts.tar /etc/scripts /etc/scripts2/test1.sh

```

This will create a tar file named **scripts.tar** containing directory and files within **/etc/scripts** and including file **/etc/scripts2/test1.sh**

## Example 2: List contents of archive file


```

tar -tvf scripts.tar

```

using the **-t** option we will list the contents. **-v** for verbose and **-f** for specifying the file **scripts.tar**

## Example 3: Append or add files to end of archive or tar file.


```

tar -rvf scripts.tar /etc/scripts2/test2.sh

```

With the **-r** option we append the file **test2.sh** into **scripts.tar**

## Example 4: Extracting files and directories from tar

```

tar -xvf scripts.tar

```

Using the **-x** option we extract the file

## Example 5: Extracting to specified directory


```

tar -xvf scripts.tar -C /tmp/

```

Using the **-C** option we specified the directory to **/tmp/**


## Example 6: Compressing further with gzip

```

tar -zvf scripts2.tar.gz /etc/scripts2/

OR


tar -zpvf scripts2.tar.gz /etc/scripts

```

The **-z** option will compress with gzip and the **-p** option will save permissions of files.

## Example 7: Compressing further with bzip2

```

tar -jvf etc.tar.bz2 /etc/

OR

tar -jpvf etc.tar.bz2 /etc/

```

The **-j** option will compress with gzip and the **-p** option will save permissions of files.

## Example 8: Excluding particular files or type while creating tar file.

```
tar -zpvf test.tar.gz /etc/ /opt/ --exclude=*.html

```
The **--exclude** option will exclude certain files ending with **.html**

## Example 9: List contents

```

tar -tvf test.tar.gz

```

With **-t** option we get an example to list everything withing **test.tar.gz**




[< Back: sed and awk](https://github.com/sxcdennis/Linux-Guides/blob/master/sedawk.md "sed and awk") || [Next: Permissions: chmod, chown, chgrp & setfacl >](https://github.com/sxcdennis/Linux-Guides/blob/master/chmodacl.md "Permissions chmod & ACL")
