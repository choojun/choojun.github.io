# M. Hive Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. It is a distributed, fault-tolerant data warehouse system that enables analytics at a massive scale and facilitates reading, writing, and managing petabytes of data residing in distributed storage using SQL
2. Read more on Hive at URL [https://hbase.apache.org/book.html](https://cwiki.apache.org/confluence/display/Hive/GettingStarted)

## M1. Install and Configure Hive [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser. Ensure the following services started sequentially and check them with command jps
    - SSH
    - HDFS
    - YARN

2. Install Hive
~~~bash
$ cd ~
$ wget  https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
$ tar -xvzf apache-hive-3.1.3-bin.tar.gz
$ mv apache-hive-3.1.3-bin hive
~~~

3. Edit the file ~/.bashrc with the the following lines 
~~~bash
 export HADOOP_MAPRED_HOME=${HADOOP_HOME}
 export HADOOP_COMMON_HOME=${HADOOP_HOME}
 export HADOOP_HDFS_HOME=${HADOOP_HOME}
 export YARN_HOME=${HADOOP_HOME}
 export HIVE_HOME=/home/hduser/hive
 export PATH=$PATH:$HIVE_HOME/bin
 export CLASSPATH=$CLASSPATH:$HADOOP_HOME/lib/*:$HIVE_HOME/lib/*
~~~

4. Re-load the environment
~~~bash
$ source ~/.bashrc
~~~

5. Duplicate the file $HIVE_HOME/conf/hive-site.xml 
~~~bash
$ cd $HIVE_HOME
$ cp conf/hive-default.xml.template conf/hive-site.xml
~~~

6. Edit the file $HIVE_HOME/conf/hive-site.xml
* Replace all occurrences of ${system:java.io.tmpdir} to /tmp/hive and this location is the new location for Hive storing all its temporary files.
* Replace all occurrences of ${system:user.name} to ${user.name} which should be the user name you log in with
* Replace all occurrences of /user/hive tp /user/hduser
> You may want to use nano's search (Ctrl-w with Enter) to find the targeted keyword, and replace them one by one with typing.

7. Create Hive directories in the HDFS
~~~bash
$ hdfs dfs -mkdir /tmp
$ hdfs dfs -mkdir /tmp/hduser
$ hdfs dfs -mkdir /user
$ hdfs dfs -mkdir /user/hduser
$ hdfs dfs -mkdir /user/hduser/warehouse
$ hdfs dfs -mkdir /user/hduser/lib
$ hdfs dfs -chmod g+w /tmp
$ hdfs dfs -chmod g+w /tmp/hduser
$ hdfs dfs -chmod g+w /user
$ hdfs dfs -chmod g+w /user/hduser
$ hdfs dfs -chmod g+w /user/hduser/warehouse
$ hdfs dfs -chmod 777 /user/hduser/lib
~~~

7. Change mode of access for hadoop's data and namenode, which are created in Section 
~~~bash
$ chmod -R g+w /home/hduser/hadoopData
$ chmod -R g+w /home/hduser/hadoopName
~~~


8. Delete the log4j-slf4j-impl-2.17.1.jar file (optional)
~~~bash
$ rm $HIVE_HOME/hive/lib/log4j-slf4j-impl-2.17.1.jar
~~~
> We delete the file log4j-slf4j-impl-2.17.1.jar because the similar file is also presented in the Hadoop directory, and it gives error to us occasionally



### Steps 7 - 10 are useful to setup the Derby in Server Mode. 
> For pseudo-distributed mode, kindly refer to the Hive-Installation section at this link: https://www.tutorialspoint.com/hive/hive_installation.htm

## M2. Install and Configure Derby [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Install and configure Derby
~~~bash
$ cd ~
$ wget https://archive.apache.org/dist/db/derby/db-derby-10.14.2.0/db-derby-10.14.2.0-bin.tar.gz
$ tar -xzf db-derby-10.14.2.0-bin.tar.gz
$ mv db-derby-10.14.2.0-bin derby 
$ mkdir derby/data
~~~

2. Edit the file /etc/profile.d/derby.sh with the the following lines
~~~bash
$ cd ~
~~~

