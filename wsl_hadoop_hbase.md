# K. HBase Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Read more on HBase at URL https://hbase.apache.org/book.html

## K1. Install HBase [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, and check the installed scala package
~~~bash
$ cd ~/hadoop3
$ sbin/start-dfs.sh
$ sbin/start-yarn.sh
$ cd ~
~~~

2.	Check the installed version of Hadoop
~~~bash
$ cd ~
$ hadoop version
~~~
Reinstall the Hadoop if it is not version 3.3.6. Remember to delete the existing hadoop3 directory before begin your setup.

3.	Check the installed version of PySpark
~~~bash
$ pyspark --version
~~~
Reinstall the Spark if it the PySpark not version 3.5.0 (for Hadoop 3.3.6 and later). Remember to delete the existing spark directory before begin your setup.

4.	Check the installed version of Scala
~~~bash
$ scala -version
$ ll ~/kafka/libs | grep kafka
~~~
Reinstall the Kafka if it is not those version of your install Scala, say 2.13.x as shown in file kafka_<Scala-version>-<Kafka-version>.*. Remember to delete the existing kafka directory before begin your setup.







