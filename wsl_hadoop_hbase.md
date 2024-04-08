![hbase](https://github.com/choojun/choojun.github.io/assets/6356054/bb45df8d-cfca-4cf2-abf1-9520ee683a4e)

# K. HBase Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. HBase is a type of NoSQL database. NoSQL is a general term meaning that the database isn’t an RDBMS which supports SQL as its primary access language, but there are many types of NoSQL databases: BerkeleyDB (https://en.wikipedia.org/wiki/Berkeley_DB) is an example of a local NoSQL database, whereas HBase is very much a distributed database. 
2. HBase has many features which supports both linear and modular scaling. HBase clusters expand by adding RegionServers that are hosted on commodity class servers. If a cluster expands from 10 to 20 RegionServers, for example, it doubles both in terms of storage and as well as processing capacity. An RDBMS can scale well, but only up to a point - specifically, the size of a single database server - and for the best performance requires specialized hardware and storage devices.
3. Read more on HBase at URL https://hbase.apache.org/book.html

## K1. Install HBase [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser. Ensure the following services started sequentially and check them with command jps
    - SSH
    - HDFS
    - YARN

2.	Check the installed version of Hadoop
~~~bash
$ cd ~
$ hadoop version
~~~
> Reinstall the Hadoop if it is not version 3.3.6. Remember to delete the existing hadoop3 directory before begin your setup.

3.	Check the installed version of PySpark
~~~bash
$ pyspark --version
~~~
> Reinstall the Spark if it the PySpark not version 3.5.0 (for Hadoop 3.3.6 and later). Remember to delete the existing spark directory before begin your setup.

4.	Check the installed version of Scala
~~~bash
$ scala -version
$ ll ~/kafka/libs | grep kafka
~~~
> Reinstall the Kafka if it is not those version of your install Scala, say 2.13.x as shown in file kafka_(Scala-version)-(Kafka-version).*. Remember to delete the existing kafka directory before begin your setup.

5.	Download HBase
~~~bash
$ cd ~
$ wget https://archive.apache.org/dist/hbase/2.5.7/hbase-2.5.7-bin.tar.gz
$ tar -xvzf hbase-2.5.7-bin.tar.gz
$ mv hbase-2.5.7 hbase
~~~
> Find the current stable release of HBase that is compatible with your version of Hadoop at here (https://hbase.apache.org/book.html#hadoop), and you may find a list of releases at Apache Hbase download page (https://hbase.apache.org/downloads.html). For example, you may adopt version Hbase 2.5.x for the installed Hadoop 3.3.6.


## K2. Configure HBase [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Stop (if running) the Zookeeper and Kafka services
~~~bash
$ cd ~/kafka
$ bin/kafka-server-stop.sh
$ bin/zookeeper-server-stop.sh
$ jps
~~~
> Attention! Please wait at least 30 seconds after issuing each command. Responses might be slow, and do use the command jps to observe the termination of services, i.e. **HQuorumPeer** and **Kafka**.

2.	Edit the Bash profile of hduser by adding the following lines, and source the profile
~~~bash
export HBASE_HOME=/home/hduser/hbase
export PATH=$HBASE_HOME/bin:$PATH
~~~

3.	Set environment variables in ~/hbase/conf/hbase-env.sh by uncomment and edit the environment variables
~~~bash
  export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
  export HBASE_HOME=/home/hduser/hbase
  export HBASE_CLASSPATH=${HBASE_HOME}/lib
  export HBASE_REGIONSERVERS=${HBASE_HOME}/conf/regionservers
  export HBASE_MANAGES_ZK=false
~~~
> Please start both zookeeper and kafka servers before this step. If you are planning to run the Kafka service, then in hbase-env.sh change HBASE_MANAGES_ZK from true to false

4. Edit and add the properties of ~/hbase/conf/hbase-site.xml 
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

5. Start the Zookeeper and Kafka services
~~~bash
$ cd ~/kafka
$ bin/zookeeper-server-start.sh config/zookeeper.properties &
$ bin/kafka-server-start.sh config/server.properties &
~~~
> Attention! Please wait at least 30 seconds after issuing each command. Responses may be slow to start following your recent configuration.
> Please verify that Zookeeper and Kafka are running by running the jps command, i.e. you should see the process: **HQuorumPeer** and **Kafka**, before proceed to the next step. 

6. Start the HBase
~~~bash
$ cd ~
$ ~/hbase/bin/start-hbase.sh
$ jps
~~~
> Verify that HBase is running - you should see the HBase processes with jps command, i.e. **HMaster** and **HRegionServer**. Note that you may ignore the errors due to SLF4J during HBase startup, as it will eventually perform logging to a plain text in local file system.
> When you want to stop HBase, type the following commands 
~~~bash
$ cd ~
$ ~/hbase/bin/stop-hbase.sh
$ jps
~~~
> You may choose to clear all the HBase data, after stopped both HMaster and HRegionServer, using the following commands
~~~bash
$ hdfs dfs -ls /
$ hdfs dfs -rm -r /hbase
$ hdfs dfs -ls /
~~~
> Delete the log4j-slf4j-impl-2.17.2.jar file (optional)
~~~bash
$ rm ~/hive/lib/log4j-slf4j-impl-2.17.2.jar
~~~
> We delete the file log4j-slf4j-impl-2.17.2.jar because the similar file is also presented in the Hadoop directory, and it gives error to us occasionally

## K3. Using HBase Shell [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, say in another session, after HBase started
~~~bash
$ cd ~
$ ~/hbase/bin/hbase shell
~~~

2.	Get a listing of commands
~~~bash
hbase(main):001:0> help
~~~

3.	Check the status of the HBase cluster
~~~bash
hbase(main):002:0> status
~~~

4.	To view all tables
~~~bash
hbase(main):003:0> list
~~~

5.	To exit the HBase Shell
~~~bash
hbase(main):004:0> exit
~~~



## K4. Running HBase Commands [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Create a table named linkshare in the default namespace with one column-family called link
~~~bash
hbase> create 'linkshare', 'link'
~~~

2. Add a new column-family named statistics to the table. 
~~~bash
hbase> disable 'linkshare'
hbase> alter 'linkshare', 'statistics'
hbase> enable 'linkshare'
~~~
> Note that to alter the table (e.g. change column-family, add column-family, etc) after it has been created, you need to first disable the table to prevent clients from assessing the table during the alter operation.

3. Verify that the new column-family has been added to the table
~~~bash
hbase> describe 'linkshare'
~~~

4. Insert a value in a cell at the specified table/row/column, use the command put. In the following examples, we use the unique reversed URL link for the row keys
~~~bash
hbase> put 'linkshare', 'org.hbase.www', 'link:title', 'Apache HBase'
hbase> put 'linkshare', 'org.hadoop.www', 'link:title', 'Apache Hadoop'
hbase> put 'linkshare', 'com.oreilly.www', 'link:title', 'O\'Reilly.com'
~~~


5. For incrementing frequency counters, use the command incr. In the examples below, we indicate that the counter should be incremented by 1
~~~bash
hbase> incr 'linkshare', 'org.hbase.www', 'statistics:share', 1
hbase> incr 'linkshare', 'org.hbase.www', 'statistics:like', 1
hbase> incr 'linkshare', 'org.hbase.www', 'statistics:share', 1
~~~

6. To access a counter’s current value, use the get_counter command, specifying the table name, row key, and column
~~~bash
hbase> get_counter 'linkshare', 'org.hbase.www', 'statistics:share'
~~~

7. To perform lookups by row key to retrieve attributes for a specific row, use the get command
~~~bash
hbase> get 'linkshare', 'org.hbase.www'
~~~
> The get command also accepts an optional dictionary of parameters to specify the column(s), timestamp, timerange, and version of the cell values to be retrieved. e.g.
~~~bash
hbase> get 'linkshare', 'org.hbase.www', 'link:title'
hbase> get 'linkshare', 'org.hbase.www', 'link:title', 'statistics:share'
hbase> get 'linkshare', 'org.hbase.www', ['link:title', 'statistics:share']
hbase> get 'linkshare', 'org.hbase.www', {TIMERANGE => [1399887705673, 1400133976734]}
hbase> get 'linkshare', 'org.hbase.www', {COLUMN => 'statistics:share', VERSIONS => 2}
~~~

8. Display all the contents of a table
~~~bash
hbase> scan 'linkshare'
~~~


9. Limit scan to the rows starting with a specific row key
~~~bash
hbase> scan 'linkshare', {COLUMNS => ['link:title'], STARTROW => 'org.hbase.www'}
hbase> scan 'linkshare', {COLUMNS => ['link:title'], STARTROW => 'org'}
~~~


10. Import the necessary classes
~~~bash
hbase> import org.apache.hadoop.hbase.util.Bytes
hbase> import org.apache.hadoop.hbase.filter.SingleColumnValueFilter
hbase> import org.apache.hadoop.hbase.filter.BinaryComparator
hbase> import org.apache.hadoop.hbase.filter.CompareFilter
~~~
> Create a filter that limits the results to rows where the statistics:like counter column value is less than or equal to 10
~~~
     likeFilter = SingleColumnValueFilter.new(Bytes.toBytes('statistics'),
     Bytes.toBytes('like'),
     CompareFilter::CompareOp.valueOf('GREATER_OR_EQUAL'),
     BinaryComparator.new(Bytes.toBytes(10)))
~~~
> Set a flag for the filter to skip any rows without a value in this column
~~~bash
hbase> likeFilter.setFilterIfMissing(true)
~~~
> Run a scan with the configured filter
~~~bash
hbase> scan 'linkshare', { FILTER => likeFilter }
~~~

## K5.  Data Definition Language (DDL) Commands [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Create a table named 't1' with a column family 'cf1'
~~~bash
hbase> create 't1', 'cf1'
~~~

2. Create a table named 'emp', with two column families: 'personal data' and 'professional data'
~~~bash
hbase> create 'emp', 'personal data', 'professional data'
hbase> list
~~~

3. The describe command
~~~bash
hbase> describe 'emp'
~~~

4. Create a namespace 'ns1'
~~~bash
hbase> create_namespace 'ns1'
~~~

5. Create a table named 't1' in the namespace 'ns1' with a column family cf1' and maximum of 5 versions of all columns in the column family 'cf1'
~~~bash
hbase> create 'ns1:t1', {NAME=>'cf1', VERSIONS=>5}
hbase> list
~~~

6. Create a table named 't2' in the namespace 'ns1' with two column families 'cf1' and 'cf1'
~~~bash
hbase> create 'ns1:t2', 'cf1', 'cf2'
hbase> list
hbase> describe 'ns1:t2'
~~~

7. The alter command. Add additional column families 'cf3', 'cf4,  and 'cf5' to the table 'n1:t2'
~~~bash
hbase> alter 'ns1:t2', 'cf3', 'cf4', 'cf5'
hbase> describe 'ns1:t2'
~~~

8. Delete the  column family 'cf3' of the table 'ns1:t2''
~~~bash
hbase> alter 'ns1:t2', NAME=>'cf3', METHOD=>'delete'
~~~

9. Delete the  column family 'cf4' of the table 'ns1:t2'
~~~bash
hbase> alter 'ns1:t2', 'delete'=>'cf4'
hbase> describe 'ns1:t2'
~~~

10. Change the maximum number of versions of the columns in a column family
~~~bash
hbase> alter 'emp', {NAME=>'personal data', VERSIONS=>5}
~~~

## K6.  Data Manipulation Language (DML) Commands [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)


## K7. Attention: At the beginning of all future practical [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Login as hduser or switch account to hduser
~~~bash
$ su - hduser
~~~

2. Start ssh,
~~~bash
$ sudo service ssh start
~~~

3. Start the HDFS service
~~~bash
$ sbin/start-dfs.sh
$ jps
~~~
> Suppose you need to observe at least four (4) services, including both NameNode and DataNode, as stated in step 12 of G7. Otherwise, you may need to reformat the HDFS NameNode.
>  ~~~bash
> $ cd ~/hadoop3
> $ bin/hdfs namenode -format
> $ sbin/start-dfs.sh
> $ hdfs dfs -mkdir /user
> $ hdfs dfs -mkdir /user/hduser
> ~~~

4. Start the Yarn service
~~~bash
$ sbin/start-yarn.sh
$ jps
~~~
> Suppose you need to observe at least six (6) services in the total, refer steps 12 and 13 of G7.

5. Start the Zookeeper and Kafka services
~~~bash
$ cd ~/kafka
$ bin/kafka-server-stop.sh
$ bin/zookeeper-server-stop.sh
$ bin/zookeeper-server-start.sh config/zookeeper.properties &
$ bin/kafka-server-start.sh config/server.properties &
~~~
> Please wait at least 30 seconds after issuing each command. Note that responses may be slow.
> Suppose you need to observe at least eight (8) services in the total, including both HQuorumPeer and Kafka, as stated in step 6 of K2.

6. Start the HBase
~~~bash
$ cd ~
$ $HBASE_HOME/bin/start-hbase.sh
$ jps
~~~
> Suppose you need to observe at least ten (10) services in the total, including both HMaster and HRegionServer, as stated in step 7 of K2.







## K8. Attention: At the end of all future practical [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)


1. Stop the HBase service
~~~bash
$ cd ~
$ $HBASE_HOME/bin/stop-hbase.sh
$ jps
~~~
> Suppose the two services will be terminated, i.e. HMaster and HRegionServer.


2. Stop (if running) the Zookeeper and Kafka services
~~~bash
$ cd ~/kafka
$ bin/kafka-server-stop.sh
$ bin/zookeeper-server-stop.sh
~~~
> Attention! Please wait at least 30 seconds after issuing each command. Responses may be slow.
> Suppose the two services will be terminated, i.e. HQuorumPeer and Kafka


3. Stop the YARN and HDFS services
~~~bash
$ cd ~/hadoop3
$ sbin/stop-yarn.sh
$ sbin/stop-dfs.sh
$ top
~~~
> Ctrl-c to terminate the command top

4. Exit from your user account(s) 
~~~bash
exit
~~~


