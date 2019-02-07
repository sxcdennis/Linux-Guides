# Basic MySQL / MariaDB

In this section we'll go over 4 major topics for basics of MYSQL/ MariaDB :

- Install and configure Maria DB

- Create a simple database schema

- Backup and restore database

- Perform simple SQL queries against a database


# Install and configure Maria DB

**1.** To install mysql/mariaDB package use (mariadb for centos/redhat)

```

yum -y install mariadb mariadb-server

```


**2.** Enable Services


```

systemctl start mariadb
systemctl enable mariadb

```

**3.** Run clean install

```

mysql_secure_installation

```

It will go through some options... like configuring passwords, remove anonymous users

![mysql1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql**1.**png?raw=true)




**4.** Permit mysql in firewall

```

firewall-cmd --permanent --add-service=mysql
firewall-cmd --reload

```


# Tuning MySQL ( Optional )

MySQL Tuner is a Perl script that connects to a running instance of MySQL and provides configuration recommendations based on workload. Ideally, the MySQL instance should have been operating for at least 24 hours before running the tuner. The longer the instance has been running, the better advice MySQL Tuner will give.

****1.**** Download MySQL Tuner to your home directory.

``wget https://raw.githubusercontent.com/major/MySQLTuner-perl/master/mysqltuner.pl``


**2.** Run it

`perl ./mysqltuner.pl`





# Create a simple database schema

In this section we can go over creating databases, deleting databases and creating users for databases

****1.**** Login as root with password you set up

```

mysql -p -u root

```


****2.**** Create a database named **test1**

```

create database test1;

```

**3.** Delete database **test1**


```
drop database test1;

```

**4.** There are multiple ways to **create users**

**4a.** Create user **newuser** with password **password** at domain **localhost**

```
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```


**4b.** Grants user **testuser** with the password **password** on database **databasename** at domain **localhost**

```

grant all on databasename.* to 'testuser'@'localhost' identified by 'password';

```

The previous example will also create the database **databasename** if it hasn't been created.


# Backup and restore database



# Perform simple SQL queries against a database
