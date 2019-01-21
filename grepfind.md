# grep & find

**find** finds files within a given directory. **grep** finds text and displays it in regular expression. Both of these tools can go very deep but I will just cover the basics of them.


# grep

grep stands for "global regular expression print" which processes text line by line and prints lines that match specified pattern(s). It is often used while pipe'd "|"



**Syntax**

```

grep [OPTIONS] PATTERN [FILE]


```

**Common Options**

```
-E : "(patter1|pattern2|patternx)" : Extended grep that will each multiple patterns
-F : Fixed string patterns, separated by new lines.
-i: Ignore case sensitive letters.
-r: Recursive output.
-A NUM: Print NUM lines of trailing context AFTER matching lines.
-B NUM: Print NUM lines of leading context BEFORE matching lines.
-C NUM: Print NUM of lines of output context.

```

**Grep examples**

```

grep  dennis /etc/example

man grep| grep -Ei "(Ignore|Case|rEcuSiVe|FiXed)"

man cd| grep -A 2 find

man ls| grep -C 1 line

ps -aux| grep -rB 2 bash


```


# find

find searches for files in a directory hierarchy.

**Syntax**

```

find [options] [path] [tests] expression [action]

```

**Common Tests**

```

-gid n: Returns true if files match GID(n)
-group gname: Returns true if belongs to gname
-name pattern: Returns true if base of the file name matches pattern.
-nogroup: True if no group
-nouser: True if no uesr
-path pattern: Path to directory
-size [-|+]n[cwbkMG]: - is less than + is more than b-blocks,c-bytes,w-words,k-KB,M-MB,G-GB
-type typesoffiel- typeoffile: b-block, c-character, d-directories, p-pipes, f-file, l-symbolic link
-uid n : if n(uid) matches



```


**Common Actions**


```

-delete: Deletes matches files
-exec command '{}' \; Executes command on each matched file.
-exec command1 '{}' command2 '{}' Executes Multiple commands on matching file
-execdir command '{}' \; Executes command on each match directory
-excdir command1 '{}' command2 '{}'  Executes Multiple commands on matching directory
-ls returns true on list

```



[< Back:User & Group Management](https://github.com/sxcdennis/Linux-Guides/blob/master/usergroup.md  "User & Group Management") || [Next: sed and awk >](https://github.com/sxcdennis/Linux-Guides/blob/master/sedawk.md "sed and awk")
