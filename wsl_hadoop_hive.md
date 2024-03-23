# M. Hive Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Read more on Hive at URL [https://hbase.apache.org/book.html](https://cwiki.apache.org/confluence/display/Hive/GettingStarted)

## M1. Install Hive [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, and start the DFS and YARN services (if not running).
~~~bash
$ su - hduser
$ jps
$ cd ~/hadoop3
$ sbin/start-dfs.sh
$ sbin/start-yarn.sh
$ jps
$ cd ~
~~~






