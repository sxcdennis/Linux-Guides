# sed & awk  

sed is a stream editor that is used to perform some basic text transformation. awk is mainly used to find patterns and manipulate text. Both of these tools can get quite complex but we'll just go over some of the basics of them.

# sed

**syntax1**


```

sed [options] /path/to/file

```


**matching strings**

*'s/string1/stirng2/'* will match string1 and replace with string2

**Example:**

```

echo "Hello my name is Dennis" | sed 's/Hello/Yoyo/''

```

You can do this with files as well.

```

echo "Hello my name is Dennis" > test.txt | sed 's/Hello/Yo yo yo/' ./test.txt

```


**Matching Multiple Strings**
You can match multiple strings by using the -e options followed by *;* to separate each one.

**Example**

```

echo "Yo my dude"| sed -e 's/Yo/Hey/; s/dude/Man/'


```


You can have more sed options with actions.

**syntax2**

```

sed /s/pattern/replacement/action


```

**actions**


**g** replace all occurrences

**A num** The occurrence of number for new text you want to subtitle

**p** Print original content

**w file** Writes results to a file



**Replace Characters**

```

sed 's!/pattern!/replacement!' /path/to/file


```



**Limiting sed**

You can limit sed to certain amount of lines.

```
sed 'nums/pattern/replacement/' /path/to/file

```

You can do ranges with the *,* comma. Example: **sed '2,3s/test/another test'**

**Delete Lines**

To delete lines we can use *d* option.

EXAMPLE:

```

sed '2d' myfile
sed '2,3d' myfile
sed '/test 1/d' my file
sed '/first/, /rangetwo/d' mfile

```

# awk


```

awk [options] file

```

**Common options:**

**-F "fs"** Field Separator

**-f file**  To specify a file that contains awk script.

**-v var=value** To declare a variable.

## Printing variables

```

awk '{print $1 $2 $n} path/to/file'

```

## Using Multiple Commands

You can use multiple commands by adding a semicolon as long as its within the curly-braces.

```

echo "Hello Sir" | awk '{$2="Dude"; print $0}'

```

## Reading The Script From File.

You can type your awk script in a file then specify it using the -f option.

Lets say your script contains.

**test1.awk**

```
{print $4}

```

Then run the command:

```

awk -F: -f ./test1.awk  /etc/services


```

## Print variables

You can print defined variables using -v

Example:

```

echo | awk -v shelln=$0 '{print "My shell is" shelln}'


```

# Summary

These are just the basics of awk, and sed but it can get much deeper than this. 



[< Back: grep and find](https://github.com/sxcdennis/Linux-Guides/blob/master/grepfind.md "grep and find") || [Next:tar >](https://github.com/sxcdennis/Linux-Guides/blob/master/tar.md "tar")
