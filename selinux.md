# SELinux modes

SELinux runs in one of three modes or states.

**Permissive**

This is a diagnostic state. The security policy rules are not enforced, but SELinux sends denial messages to a log file. This allows you to see what would have been denied if SELinux were running in enforcing mode.

**Enforcing**

This is the default state that enforces SELinux security policy. Access is denied to users and programs unless permitted by SELinux security policy rules. All denial messages are logged as AVC (Access Vector Cache) Denials.

**Disabled**

SELinux does not enforce a security policy because no policy is loaded in the kernel. Only DAC rules are used for access control.

# Commands for SELinux

**sestatus :** Gets current SELinux Status

**setenforce 0 :** Sets current mode to permissive. (Does not apply permanently you must edit config file)

**setenforce 1 :** Sets current mode to enforcing. (Does not apply permanently you must edit config file)

**setenforce enforcing :** Sets current mode to enforcing. (Does not apply permanently you must edit config file)

**setenforce permissive :** Sets current mode to permissive. (Does not apply permanently you must edit config file)


To make this change permanent, edit the /etc/sysconfig/selinux file (or the /etc/selinux/config file)

Replace type with either enforcing,permissive, or disabled:

```

SELINUX=type


```

[< Back: firewall](https://github.com/sxcdennis/Linux-Guides/blob/master/firewall.md "firewall") || [Next: Filesystems >](https://github.com/sxcdennis/Linux-Guides/blob/master/filesystems.md "Filesystems")
