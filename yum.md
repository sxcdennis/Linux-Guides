# yum: Creating and Updating repositories (including updating kernels)

**yum** and **rpm** are both able to install and upgrade packages. The biggest difference is that *YUM* looks for dependencies whereas *rpm* does not. This means that rpm can install any versions whereas

# yum


## Common Options

**-y** Assumes yes if prompted

**--assumeno** No if prompted

**--disablerepo=reponame**

**--downloadonly** downloads but doesn't install.

**whatprovides name** searches dependencies for name

**install** installs package from a repository to your system

**update** Updates all or one package on your system

**update-to** Updates to a particular version

**upgrade** Update packages taking obsoletes into account

**downgrade** Downgrade to earlier version

**reinstall** Reinstall the current version

**swap package1 package2** Removes package1 for package2

**remove** removes package from system

**groupinstall** Install packages that are selected as a group.

**clean** Clear out cached package data.

**yum clean all** - Clean out all packages and metadata from cache



# rpm

## Common Options


**rpm -ivh {rpm-file}**	Install the package

**rpm -Uvh {rpm-file}**	Upgrade package

**rpm -ev {package}**	Erase/remove/ an installed package

**rpm -ev --nodeps {package}**	Erase/remove/ an installed package without checking for dependencies


**rpm -qa**	Display list all installed packages

**rpm -qi {package}**	Display installed information along with package version and short description	rpm -qi mozilla-mail

**rpm -qf {/path/to/file}**	Find out what package a file belongs to i.e. find what package owns the file	rpm -qf /etc/passwd


**rpm -qc {pacakge-name}**	Display list of configuration file(s) for a package

**rpm -qcf {/path/to/file}**	Display list of configuration files for a command

**rpm -qa --last**	Display list of all recently installed RPMs

**rpm -qR {package}**	Find out what dependencies a rpm file has





# Installing Custom repositories

1. To install custom repositories with yum you must edit or create a **.repo** fine in  **/etc/yum.repos.d/**
And add contents to it:

```

[nameofrepo]
name=nameofrepo
baseurl=http://server.example.com/repo
gpgcheck=0
enabled=1


```

You can replace **nameofrepo** to anything and your baseurl can even be a path to a repo in your local computer.

2. You then use **yum clean all** to clear cache

3. Then excute **yum repolist** to view your new repo name.

4. Then excute **yum install newreponame**


**Note** This whole process works for installing kernels as well.





[< Back: Creating & Maintaining LVM](https://github.com/sxcdennis/Linux-Guides/blob/master/lvm.md "Creating & Maintaining LVM") || [Next: Configuring & Installing KVM on CentOS 7.0 >](https://github.com/sxcdennis/Linux-Guides/blob/master/Configuring%20%26%20Installing%20KVM%20on%20CentOS%207.md "Configuring & Installing KVM on CentOS 7.0 ")
