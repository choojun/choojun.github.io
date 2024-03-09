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


5.	Download HBase
~~~bash
$ cd ~
$ wget https://archive.apache.org/dist/hbase/2.5.7/hbase-2.5.7-bin.tar.gz
$ tar -xvzf hbase-2.5.7-bin.tar.gz
$ mv hbase-2.5.7 hbase
~~~
Find the current stable release of HBase that is compatible with your version of Hadoop at here (https://hbase.apache.org/book.html#hadoop), and you may find a list of releases at Apache Hbase download page (https://hbase.apache.org/downloads.html). For example, you may adopt version Hbase 2.5.x for the installed Hadoop 3.3.6.


## K2. Configure HBase [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Edit the Bash profile of hduser by adding the following lines, and source the profile
~~~bash
export HBASE_HOME=/home/hduser/hbase
export PATH=$HBASE_HOME/bin:$PATH
~~~

2.	Set environment variables in $HBASE_HOME/conf/hbase-env.sh by uncomment and edit the environment variables
~~~bash
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HBASE_HOME=/home/hduser/hbase
export HBASE_CLASSPATH=${HBASE_HOME}/lib
export HBASE_REGIONSERVERS=${HBASE_HOME}/conf/regionservers
export HBASE_MANAGES_ZK=false
~~~
Please start both zookeeper and kafka servers before this step.
If you are planning to run the Kafka service as well, then in hbase-env.sh change HBASE_MANAGES_ZK from true to false, i.e.: export HBASE_MANAGES_ZK=false

3. Add/Edit the properties in the configuration file $HBASE_HOME/conf/hbase-site.xml
~~~xml
<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.rootdir</name>
    <value>hdfs://localhost:9000/hbase</value>
  </property>
  <property>
    <name>hbase.wal.provider</name>
    <value>filesystem</value>
  </property>
</configuration>
~~~

4. Ensure that HDFS and YARN processes are already running, and you may check the required six services with following command
~~~bash
$ jps
~~~

5. Start / Restart (if running) the Zookeeper and Kafka:
~~~bash
$ cd kafka
$ bin/kafka-server-stop.sh
$ bin/zookeeper-server-stop.sh
$ bin/zookeeper-server-start.sh config/zookeeper.properties &
$ bin/kafka-server-start.sh config/server.properties &
~~~
Attention! Please wait at least 30 seconds after issuing each command. Responses may be slow to start following your recent configuration.

6. Warning! Please verify that Zookeeper and Kafka are running by running the jps command, i.e. you should  see the process: HQuorumPeer and Kafka, before proceed to the next step. 

7. Start the HBase
~~~bash
$ cd ~
$ $HBASE_HOME/bin/start-hbase.sh
$ jps
~~~
Verify that HBase is running - you should see the HBase processes with jps command, i.e. HMaster and HRegionServer. When you want to stop HBase, type the following:
~~~bash
$ cd ~
$ $HBASE_HOME/bin/stop-hbase.sh
$ jps
~~~

## K3. Using HBase Shell [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, say in another session, after HBase started
~~~bash
$ $HBASE_HOME/bin/hbase shell
~~~

2.	Get a listing of commands:
~~~bash
hbase(main):003:0> help
~~~

3.	Check the status of the HBase cluster: 
~~~bash
hbase(main):003:0> status
~~~

4.	To view all tables: 
~~~bash
hbase(main):003:0> list
~~~

5.	To exit the HBase Shell: 
~~~bash
hbase(main):003:0> exit
~~~
