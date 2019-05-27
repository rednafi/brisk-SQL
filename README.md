# brisk-SQL
----------------------------------------------------------------------------------------------------------------------

## Table of Contents
----------------------------------------------------------------------------------------------------------------------
- [Setting Up](#)
  - [Installation](#sub-heading)
  - [Allow Remote Access](#sub-heading-1)
  - [Start MySQL Service](#sub-heading-2)
  - [Launch at Reboot](#sub-heading-3)
  - [Start the MySQL Shell](#sub-heading-4)
  - [Set the Root Password](#sub-heading-5)
  - [View Users](#sub-heading-6)
  - [Create a Database](#sub-heading-7)
  - [Delete a Database](#sub-heading-8)
  - [Add a Database User](#sub-heading-9)
  - [Delete a Database User](#sub-heading-10)
  - [Grant Database User Permissions](#sub-heading-11)

## Setting Up
----------------------------------------------------------------------------------------------------------------------

 
### Installation

This part describes a basic installation of a MySQL 5.7 database server on Ubuntu Linux, 18.04 to be specific. 

```
sudo apt-get update
sudo apt-get install mysql-server
```

### Allow Remote Access

If you want to connect to the MySQL database from another machine, you must open a port in your server’s firewall (the default port is 3306). This is not necessary if you intend to run the application that uses MySQL on the same server.

Run the following command to allow remote access to the MySQL server:

```
sudo ufw allow mysql
```


### Start MySQL service

After the installation, the database service can be started via the following command:

```
systemctl start mysql
```

### Launch at Reboot
To launch the database server automatically after a reboot, run:
```
systemctl enable mysql
```

### Start the MySQL Shell

In the beginning, to start the MySQL shell, run:

```
sudo mysql -u root -p
```
Writing this command will bring up a password prompt. Put your system password and press return. The following MySQL shell prompt should appear:

```
mysql>
```
Press ```ctrl + D ``` to stop the server. 

### Set the Root Password

Enter the following command in the MySQL shell, replacing password with your new password:

```
UPDATE mysql.user SET authentication_string = PASSWORD('password') 
WHERE User = 'root';
```

You should see something like this in the command prompt:

```
Query OK, 1 row affected, 1 warning (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 1
```
To make the change take effect, type the following command:

```
FLUSH PRIVILEGES;
```


### View Users

MySQL stores the user information in its own database. The name of the database is ```mysql```. If you want to see what users are set up in the MySQL user table, run the following command:

```
SELECT User, Host, authentication_string FROM mysql.user;
```

You should see something like this:

```
+------------------+-----------+-------------------------------------------+
| User             | Host      | authentication_string                     |
+------------------+-----------+-------------------------------------------+
| root             | localhost | *2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19 |
| mysql.session    | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| mysql.sys        | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| debian-sys-maint | localhost | *8282611144B9D51437F4A2285E00A86701BF9737 |
+------------------+-----------+-------------------------------------------+
4 rows in set (0.00 sec)
```

### Create a Database

You can create a database named ```test_db``` via the following command:

```
CREATE DATABASE test_db;
```

List your databases via the following command:

```
SHOW DATABASES;
```

You should see something like this:
```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test_db            |
+--------------------+
5 rows in set (0.00 sec)
```

To ensure the changes:
```
FLUSH PRIVILEGES;
```

### Delete a Database

To delete a database ```test_db``` run the following command:
```
DROP DATABASE test_db,
```
```
FLUSH PRIVILEGES;
```



### Add a Database User

To create a new user (here, we created a new user named ```redowan``` with the password ```password```), run the following command in the MySQL shell:

```
CREATE USER 'redowan'@'localhost' IDENTIFIED BY 'password';
```
```
FlUSH PRIVILEGES;
```

Ensure that the changes has been saved via running ```FLUSH PRIVILEGES;```. Verify that a user has been successfully created via running the previous command:
```
SELECT User, Host, authentication_string FROM mysql.user;
```

You should see something like below. Notice that a new user named ```redowan``` has been created:
```
+------------------+-----------+-------------------------------------------+
| User             | Host      | authentication_string                     |
+------------------+-----------+-------------------------------------------+
| root             | localhost | *2470C0C06DEE42FD1618BB99005ADCA2EC9D1E19 |
| mysql.session    | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| mysql.sys        | localhost | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
| debian-sys-maint | localhost | *8282611144B9D51437F4A2285E00A86701BF9737 |
| redowan          | localhost | *0756A562377EDF6ED3AC45A00B356AAE6D3C6BB6 |
+------------------+-----------+-------------------------------------------+
```

### Delete a Database User

To delete a database user (here, I'm deleting the user-```redowan```) run:
```
DELETE FROM mysql.user
WHERE user='<redowan>'
AND host = 'localhost'

FlUSH PRIVILEGES;
```

### Grant Database User Permissions

Give the user full permissions for your new database by running the following command (Here, I provided full permission of ```test_db``` to the user ```redowan```:

```
GRANT ALL PRIVILEGES ON test_db.table TO 'redowan'@'localhost';
```

If you want to give permission to all the databases, type:

```
GRANT ALL PRIVILEGES ON *.* TO 'redowan'@'localhost';
```

```
FlUSH PRIVILEGES;
```
