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

![mysql1](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql1.png?raw=true)




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
flush privileges;

```

The previous example will also create the database **databasename** if it hasn't been created.

**5.** We now have two users we can log into: **newuser** and **testuser**. We'll try **testuser**

```

mysql -p -u testuser1


```

**6.** Show databases

```

show databases;

```

![mysql2](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql2.png?raw=true)

**7.** Now we can go into the database **test**

```

use test

```



**8.**  Creating a table

**Syntax**

```

CREATE  TABLE [IF NOT EXISTS] TableName (column_name dataType [optional parameters])

```

**CREATE TABLE** is the one responsible for the creation of the table in the database.

**[IF NOT EXISTS]** is optional and only create the table if no matching table name is found.

**colum_name** is the name of the column

**[optional parameters]** additional information about a field such as "  AUTO_INCREMENT" , NOT NULL etc

**data Type** defines the nature of the data to be stored in the field.

![datatypes](https://github.com/sxcdennis/Linux-Guides/blob/master/images/datatypes.png?raw=true)

**Note:** Creating multiple tables can be simplified by adding a comma after each parameter.


**Example:** Creating a table for a loan company


```

create table loans (name VARCHAR(45), address VARCHAR(45), amount VARCHAR(20));


```

**9.** Now we can see tables

```

show tables;

```

![mysql3](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql3.png?raw=true)


**10.** You can see the description of the table by using

```

desc tablename;


```

![mysql4](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql4.png?raw=true)



# Backup and restore database

## Backup Databases

To backup a database we use **mysqldump**


**Typical **Syntax****

```

mysqldump [options] [database_name [table_name]] > path/to/backup/file.sql

```


**1.** Back up **test**

```
mysqldump -u root -p test > loans.sql

```


![mysql5](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql5.png?raw=true)

In our example we'll backup **test**, but in the following I'll show some examples on how to use **mysqldump**

### Backup Examples


**1.** The created file contains commands that will delete, recreate, and repopulate the each of the tables associated with the database in bulk.

```

mysqldump --user=root --password=boogers --host=localhost birds > birds.sql

```
**2.** To prompt the user for the password, use

`mysqldump --user=root --password --host=localhost birds > birds.sql`

**3.** To back up all databases on the server, add the --all-databases switch

`mysqldump --user=root --password --host=localhost --all-databases > all.sql`


**4.** There are simpler options.

```
mysqldump  -u[username] -p[password] databasename > databasefilename.sql

```



## Restore Database

To restore our database we first have to destroy our content in **test** database.

**1.** Go into the **test** database and drop the table **loans**

```

mysql -u root -p
go test;
drop table loans;
show tables;

```

**2.** Now exit and restore your database with the database we created **loans.sql**

```

mysql --u root -p test<loans.sql

```

![mysql6](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql6.png?raw=true)

**3.** Now we'll go back into **test** database and see if the dropped table still exists.

```

mysql -u root -p
use test;
show tables;
desc loans;

```

![mysql7](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql7.png?raw=true)


**Note** If you restore database to a database that doesn't exist, it will not work- You must re-create a database first.


# Perform simple SQL queries against a database

Now that we've establish how to create and backup databases. Let's go over some simple queries we can do with databases.

**Commonly used**:

**CREATE** – allows the user to create databases and tables

**DROP** - allows the user to drop databases and tables

**DELETE** - allows the user to delete rows from specific  table

**INSERT** - allows the user to insert rows into specific  table

**SELECT** – allows the user to read the database

**UPDATE** - allows the user to update table rows

### Connecting to Database from kernel

You can connect directly to the database you want by adding the database name to the end of the command line.

```

mysql -u root -p test

```

![mysql8](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql8.png?raw=true)


### Insert Data

You can add data to the tables'(**loans**) columns by using **insert**

**Typical Syntax**

```

insert into tablename  SET column1 = 'value1', column2 = 'value1';

```

**OR**

```

insert tablename values('value1','value2','valuex'), ('value1', 'value2', 'valuex');

```


Lets do test this on our table **loans**
Remember our columns are **name address amount**


**Example 1.**

```
insert into loans set name = 'James', address = '123 fake st', amount ='2500.89';

```

It seems the issue with this option is that you can only do one value at a time.


**Example 2.**

```

insert loans values('Bob','456 Your Ave','$2050.55'), ('Joe', '789 Seventh St', '$50,000');

```

As you can see in example 2, you can insert more than one row at a time.


## Selection

You can read the database by using **select** . The **Syntax** for select can be quite long but we'll go over a simple version of it.

**Simple **Syntax****

`select [column] [FROM table] [WHERE where_condition] [ORDER BY column ASC|DESC]`


- **column** Column name

- **FROM table** - replace databasename

- **WHERE where_condition:** There are several where_conditions some simples are: where name=jack or salary >= 60000

- **ORDER BY column ASC|DESC** You can order it by columns ascending or descending.


### Selection Examples

**1.** Select rows greater than or equal to 3

```

select name from loans WHERE amount >=3 order by name ASC;
select * from loans WHERE amount >=3 order by name ASC;

```

![mysql9](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql9.png?raw=true)

As you can see, it selects all rows that have the amount **>=3**. You can choose to display all the columns or just display specific ones like **names**


**2.** Select column amount and display it in descending order.

```

select amount from loans order by amount DESC;

```

![mysql10](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql10.png?raw=true)


**3.** Select column amount and display it in ascending order.

```

select amount from loans order by amount ASC;

```

![mysql11](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql11.png?raw=true)


**4.** Select all columns where rows are with the name Bob

```

select * from loans where name= 'Bob';

```

![mysql12](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql12.png?raw=true)

## Update/ Replace data

**Update** replaces data


**Syntax**

`UPDATE 'table_name' SET 'column_name' = 'new_value' [WHERE condition];
`


### Update Examples

**1.** Change Josh to Joe

`update loans set name = 'Joe' where name= 'Josh'; `


**2.** Revert back to Josh

`update loans set name = 'Josh' where name= 'Joe';`


**3.**  Change Joe's owed amount to **$2500**

`update loans set amount = '$2500' where name= 'Joe';`


**4.** Change Bob's owed amount to $5000

`update loans set amount ='$5000' where name='Bob';`

## Delete data

To remove data you use **delete**

**Syntax**

```

delete FROM 'table_name' [WHERE condition];

```


### Examples

**1.** Delete where name is Bob

`delete from loans where name='Bob';`

![mysql13](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql13.png?raw=true)


**2.** Delete row where address is 123 fake st


`delete from loans where address='123 fake st';`


![mysql14](https://github.com/sxcdennis/Linux-Guides/blob/master/images/mysql14.png?raw=true)

**3.** Delete row where amount is $2050.55

`delete from loans where amount='$50,000';`


[< Back: freeipa](https://github.com/sxcdennis/Linux-Guides/blob/master/freeipa.md "freeipa") | [Next: postfix >](https://github.com/sxcdennis/Linux-Guides/blob/master/postfix.md "postfix")
