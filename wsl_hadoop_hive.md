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
$ wget  https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
$ tar -xvzf apache-hive-3.1.3-bin.tar.gz
$ mv apache-hive-3.1.3-bin hive
~~~





