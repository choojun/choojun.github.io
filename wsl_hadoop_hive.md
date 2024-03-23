# M. Hive Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. It is a distributed, fault-tolerant data warehouse system that enables analytics at a massive scale and facilitates reading, writing, and managing petabytes of data residing in distributed storage using SQL
2. Read more on Hive at URL [https://hbase.apache.org/book.html](https://cwiki.apache.org/confluence/display/Hive/GettingStarted)

## M1. Install Hive [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser. Ensure the following services started sequentially and check them with command jps
    - SSH
    - HDFS
    - YARN

2.	Install Hive
~~~bash
$ cd ~
$ wget  https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
$ tar -xvzf apache-hive-3.1.3-bin.tar.gz
$ mv apache-hive-3.1.3-bin hive
~~~

3.	Edit the file ~/.bashrc with the the following lines 
~~~bash
 export HIVE_HOME=/home/hduser/hive
 export PATH=$PATH:$HIVE_HOME/bin
~~~

4. Re-load the environment
~~~bash
$ source ~/.bashrc
~~~

5. Create Hive directories in the HDFS
~~~bash
$ hdfs dfs -mkdir /tmp
$ hdfs dfs -mkdir /user
$ hdfs dfs -mkdir /user/hduser
$ hdfs dfs -mkdir /user/hduser/warehouse
$ hdfs dfs -mkdir /user/hduser/lib
$ hdfs dfs -chmod g+w /tmp
$ hdfs dfs -chmod g+w /user
$ hdfs dfs -chmod g+w /user/hduser
$ hdfs dfs -chmod g+w /user/hduser/warehouse
$ hdfs dfs -chmod 777 /user/hduser/lib
~~~

6. Delete the log4j-slf4j-impl-2.17.1.jar file
~~~bash
$ rm $HIVE_HOME/hive/lib/log4j-slf4j-impl-2.17.1.jar
~~~
> We delete the file log4j-slf4j-impl-2.17.1.jar because the similar file is also presented in the Hadoop directory, and it gives error to us occasionally

### Steps 7 - 10 are useful to setup the Derby in Server Mode. 
> For pseudo-distributed mode, kindly refer to the Hive-Installation section at this link: https://www.tutorialspoint.com/hive/hive_installation.htm

