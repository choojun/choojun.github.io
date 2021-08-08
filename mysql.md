Checking progress without disrupting the import
```
If you’re loading large data files (say with half a billion rows) and want to check the progress, you definitely don’t want to use `SELECT COUNT(*) FROM table’. This query will degrade as the size of the table grows and slowdown the LOAD process. Instead you can query:

mysql> SELECT table_rows FROM information_schema.tables WHERE table_name = 'table';

source: http://derwiki.tumblr.com/post/24490758395/loading-half-a-billion-rows-into-mysql
```



To expose MySQL to anything other than localhost you will have to have the following line
```
For mysql version 5.6 and below

uncommented in /etc/mysql/my.cnf and assigned to your computers IP address and not loopback

For mysql version 5.7 and above

uncommented in /etc/mysql/mysql.conf.d/mysqld.cnf and assigned to your computers IP address and not loopback

#Replace xxx with your IP Address 
bind-address        = xxx.xxx.xxx.xxx

Or add a bind-address       = 0.0.0.0 if you don't want to specify the IP

Then stop and restart MySQL with the new my.cnf entry. Once running go to the terminal and enter the following command.

lsof -i -P | grep :3306

That should come back something like this with your actual IP in the xxx's

mysqld  1046  mysql  10u  IPv4  5203  0t0  TCP  xxx.xxx.xxx.xxx:3306 (LISTEN)

If the above statement returns correctly you will then be able to accept remote users. However for a remote user to connect with the correct priveleges you need to have that user created in both the localhost and '%' as in.

CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypass';
CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypass';

then,

GRANT ALL ON *.* TO 'myuser'@'localhost';
GRANT ALL ON *.* TO 'myuser'@'%';

and finally,

FLUSH PRIVILEGES; 
EXIT;
```



Backup existing MySQL database with its data and TABLE&VIEW objects 
```
 mysqldump -u root -ppassword --routines --add-drop-table ingenuity > ingenuity.sql
```
    
Backup script with compression 
```
 #!/bin/bash
 datevar=`date +%Y_%m_%d`
 bkfile="mobileapps_${datevar}.sql"
 mysqldump -u root -ppassword --routines --add-drop-table mobileapps > $bkfile
 #compress type 1
 tar -czvf $bkfile.tar.gz $bkfile
 #compress type 2
 gzip $bkfile
 #deploy this script in crontab with following command (script will be opened with default editor vi or vim)
 # crontab -e
 #to list existing scheduled jobs
 # crontab -l
```

    
Create new destination database name called iid, and restore the database to destination database 
```
 mysql -u root -p iid < ingenuity.sql
 mysql> CREATE USER 'monty'@'localhost' IDENTIFIED BY 'some_pass';
 mysql> GRANT ALL PRIVILEGES ON *.* TO 'monty'@'localhost'
    ->     WITH GRANT OPTION;
 mysql> CREATE USER 'monty'@'%' IDENTIFIED BY 'some_pass';
 mysql> GRANT ALL PRIVILEGES ON *.* TO 'monty'@'%'
    ->     WITH GRANT OPTION;
 mysql> CREATE USER 'admin'@'localhost';
 mysql> GRANT RELOAD,PROCESS ON *.* TO 'admin'@'localhost';
 mysql> CREATE USER 'dummy'@'localhost';
```
    
Create user and grant its with privileges 
```
 CREATE DATABASE mydb;
 CREATE USER myuser@localhost;
 SET PASSWORD FOR myuser@localhost= PASSWORD("password");
 GRANT ALL PRIVILEGES ON mydb.* TO myuser@localhost IDENTIFIED BY 'password';
 FLUSH PRIVILEGES;
```
