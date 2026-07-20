DATABASE:
`mysql -u username -p database_name` <- A command to connect to MySQL server and access a particular database
`SHOW DATABASES;` <- Listing out all databases on the server.
`CREATE DATABASE database_name;` - Creating a database
`USE database_name;` <- Puts you into the database you want to use
`DROP DATABASE database_name;` <- Deletes the specified database and all its tables.

USERS:
`SELECT User,Host FROM mysql.user;` <- Shows all the users and their corresponding hostnames.
`CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';` <- Create a database user which uses the password.
`RENAME USER 'sample'@'localhost' TO 'newUser'@'localhost';`  <- How to rename users
`SET PASSWORD FOR 'username'@'localhost' = PASSWORD('newpassword');` <- Change password for a user.
`DROP USER 'username'@'localhost';` <- Deletes the specified user.
`GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'localhost';`  <- Grants all privileges on a specific database to the specified user. You can replace "ALL PRIVILEGES" with specific privileges like "SELECT, INSERT, UPDATE, DELETE etc". Likewise, you can `*.*` to grant privileges on all databases.
`REVOKE ALL PRIVILEGES ON database_name.* FROM 'username'@'localhost';` <- Removes the privileges.
`FLUSH PRIVILEGES;` <- Reloads the privileges from the grant tables in the MySQL database ensuring that any changes made to the users take effect immediately. 

TABLES:
`SHOW TABLES;` <- Lists all the tables in the currently selected database
`SELECT * FROM table_name;` <- Retrieves rows and columns from a specified table. We can replace * symbol with specific column names from which we wish to resetrieve columns.
`DELETE FROM table_name WHERE condition;` <- Deleting rows from the specified table that match the specified condition.
`DROP TABLE table1` <- Delete a table 

<div style="page-break-after: always;"></div>

MariaDB config file:
```
[root@host my.cnf.d]# locate my.cnf 
/etc/my.cnf 
/etc/my.cnf.d 
/root/.my.cnf 
/usr/local/lp/etc/exporters/my.cnf /var/cpanel/configs.cache/_etc_my.cnf___default_equal_before_after_space_allow_undef /var/cpanel/configs.cache/_root_.my.cnf___default_equal_before_after_space_allow_undef 

[root@host etc]# cat my.cnf 
[mysqld] performance-schema=0 
log-error=/var/lib/mysql/67-43-14-17.cprapid.com.err 
innodb_file_per_table=1 
default-storage-engine=innodb 
innodb_buffer_pool_size=128M 
innodb_log_file_size=128M 
max_connections=300 
key_buffer_size = 8M 
max_allowed_packet=268435456
open_files_limit=40000
```