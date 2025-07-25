![hive](https://github.com/choojun/choojun.github.io/assets/6356054/18bc2a47-52bc-4e0c-a0c2-5c373b9db3d3)

# M. Hive Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. It is a distributed, fault-tolerant data warehouse system enabling large-scale analytics. It facilitates reading, writing, and managing petabytes of data residing in distributed storage using SQL
2. Read more on Hive at URL https://cwiki.apache.org/confluence/display/Hive/GettingStarted and https://cwiki.apache.org/confluence/display/Hive/Home

## M1. Install and Configure Hive [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser. Ensure that the SSH, DFS, and YARN services are started sequentially. Then, examine them using the jps command

2. Install Hive
~~~bash
$ cd ~
$ wget  https://dlcdn.apache.org/hive/hive-2.3.9/apache-hive-2.3.9-bin.tar.gz
$ tar -xvzf apache-hive-2.3.9-bin.tar.gz
$ mv apache-hive-2.3.9-bin hive
~~~

3. Edit the file ~/.bashrc with the the following lines 
~~~bash
export HADOOP_MAPRED_HOME=/home/hduser/hadoop3
export HADOOP_COMMON_HOME=/home/hduser/hadoop3
export HADOOP_HDFS_HOME=/home/hduser/hadoop3
export YARN_HOME=/home/hduser/hadoop3
export HIVE_HOME=/home/hduser/hive
export PATH=$HIVE_HOME/bin:$PATH
export HIVE_CONF_DIR=/home/hduser/hive/conf
export HADOOP_CLASSPATH=/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/*:/home/hduser/hadoop3/share/hadoop/hdfs/lib/*:/home/hduser/hadoop3/share/hadoop/hdfs/*:/home/hduser/hadoop3/share/hadoop/tools/lib/*:/home/hduser/hadoop3/share/hadoop/mapreduce/*:/home/hduser/hadoop3/share/hadoop/yarn/*:/home/hduser/hadoop3/share/hadoop/yarn/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/*
export HIVE_CLASSPATH=/home/hduser/hive/lib/*
~~~

4. Re-load the environment
~~~bash
$ source ~/.bashrc
~~~

5. Duplicate for the file ~/hive/conf/hive-site.xml 
~~~bash
$ cd ~/hive
$ cp conf/hive-default.xml.template conf/hive-site.xml
~~~

6. Edit the file ~/hive/conf/hive-site.xml
* Replace all occurrences of ${system:java.io.tmpdir} to /tmp/hive and this location is the new location for Hive storing all its temporary files.
* Replace all occurrences of ${system:user.name} to ${user.name} which should be the user name you log in with
* Replace all occurrences of /user/hive tp /user/hduser
> You may want to use the nano's search (Ctrl-w with Enter) to find the targeted keyword, and replace them one by one.

7. Edit the file ~/hive/bin/hive-config.sh with the the following lines 
~~~bash
  export HADOOP_HOME=/home/hduser/hadoop3
  export SPARK_HOME=/home/hduser/spark
  export HBASE_HOME=/homehduser/hbase
  export HADOOP_MAPRED_HOME=/home/hduser/hadoop3
  export HADOOP_COMMON_HOME=/home/hduser/hadoop3
  export HADOOP_HDFS_HOME=/home/hduser/hadoop3
  export YARN_HOME=/home/hduser/hadoop3
  export HIVE_HOME=/home/hduser/hive
  export HIVE_CONF_DIR=/home/hduser/hive/conf
  export HADOOP_CLASSPATH=/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/*:/home/hduser/hadoop3/share/hadoop/hdfs/lib/*:/home/hduser/hadoop3/share/hadoop/hdfs/*:/home/hduser/hadoop3/share/hadoop/tools/lib/*:/home/hduser/hadoop3/share/hadoop/mapreduce/*:/home/hduser/hadoop3/share/hadoop/yarn/*:/home/hduser/hadoop3/share/hadoop/yarn/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/*
  export HIVE_CLASSPATH=/home/hduser/hive/lib/*
~~~

8. Duplicate for the file ~/hive/conf/hive-env.sh 
~~~bash
$ cd ~/hive
$ cp ~/hive/conf/hive-env.sh.template ~/hive/conf/hive-env.sh
~~~

9. Edit the file ~/hive/conf/hive-env.sh  with the the following lines 
~~~bash
  export HADOOP_HOME=/home/hduser/hadoop3
  export SPARK_HOME=/home/hduser/spark
  export HBASE_HOME=/homehduser/hbase
  export HADOOP_MAPRED_HOME=/home/hduser/hadoop3
  export HADOOP_COMMON_HOME=/home/hduser/hadoop3
  export HADOOP_HDFS_HOME=/home/hduser/hadoop3
  export YARN_HOME=/home/hduser/hadoop3
  export HIVE_HOME=/home/hduser/hive
  export HIVE_CONF_DIR=/home/hduser/hive/conf
  export HADOOP_CLASSPATH=/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/*:/home/hduser/hadoop3/share/hadoop/hdfs/lib/*:/home/hduser/hadoop3/share/hadoop/hdfs/*:/home/hduser/hadoop3/share/hadoop/tools/lib/*:/home/hduser/hadoop3/share/hadoop/mapreduce/*:/home/hduser/hadoop3/share/hadoop/yarn/*:/home/hduser/hadoop3/share/hadoop/yarn/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/*
  export HIVE_CLASSPATH=/home/hduser/hive/lib/*
~~~

10. Start both DFS and YARN services before create those required Hive directories in the HDFS. Stop both service after done the directories creation
~~~bash
$ hdfs dfs -mkdir /tmp
$ hdfs dfs -mkdir /tmp/hduser
$ hdfs dfs -mkdir /user
$ hdfs dfs -mkdir /user/hduser
$ hdfs dfs -mkdir /user/hduser/warehouse
$ hdfs dfs -mkdir /user/hduser/lib
$ hdfs dfs -chmod g+w /tmp
$ hdfs dfs -chmod g+w /tmp/hduser
$ hdfs dfs -chmod 777 /user
$ hdfs dfs -chmod g+w /user/hduser
$ hdfs dfs -chmod g+w /user/hduser/warehouse
$ hdfs dfs -chmod 777 /user/hduser/lib
~~~
> Tips to start-stop DFS and YARN services
> ~~~bash
> $ jps
> $ cd ~/hadoop3
> $ sbin/start-dfs.sh
> $ sbin/start-yarn.sh
> $ jps
> $ sbin/stop-yarn.sh
> $ sbin/stop-dfs.sh
> $ jps
> ~~~

11. Change mode of access for hadoop's data and namenode, which are created in Section E
~~~bash
$ chmod -R g+w /home/hduser/hadoopData
$ chmod -R g+w /home/hduser/hadoopName
~~~


12. Find the targeted jar file. Delete the log4j-slf4j-impl-2.6.2.jar file if found multiple copies (optional)
~~~bash
$ cd ~
$ find . -name log4j-slf4j-impl-*.jar
$ rm ~/hive/lib/log4j-slf4j-impl-2.6.2.jar
~~~
> We delete the file log4j-slf4j-impl-2.6.2.jar because the similar file is also presented in the Hadoop directory, and it gives error to us occasionally


## M2. Install and Configure Derby [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Install and configure Derby
~~~bash
$ cd ~
$ wget https://archive.apache.org/dist/db/derby/db-derby-10.14.2.0/db-derby-10.14.2.0-bin.tar.gz
$ tar -xzf db-derby-10.14.2.0-bin.tar.gz
$ mv db-derby-10.14.2.0-bin derby 
$ mkdir derby/data
~~~

2. Edit the file ~/.bashrc with the the following lines 
~~~bash
  export DERBY_INSTALL=/home/hduser/derby
  export DERBY_HOME=/home/hduser/derby
  export PATH=$PATH:/home/hduser/derby/bin
  export CLASSPATH=$CLASSPATH:/home/hduser/derby/lib/derby.jar:/home/hduser/derby/lib/derbytools.jar
~~~

3. Re-load the environment
~~~bash
$ source ~/.bashrc
~~~

4. Configure the Hive to use Network Derby by changing the following property values in file ~/hive/conf/hive-site.xml
~~~xml
    <property>
      <name>javax.jdo.option.ConnectionURL</name>
      <value>jdbc:derby://localhost:1527/metastore_db;create=true</value>
      <description>JDBC connect string for a JDBC metastore</description>
    </property>
    <property>
      <name>javax.jdo.option.ConnectionDriverName</name>
      <value>org.apache.derby.jdbc.ClientDriver</value>
      <description>Driver class name for a JDBC metastore</description>
    </property>
    <property>
      <name>hive.exec.local.scratchdir</name>
      <value>/tmp/hive/${user.name}</value>
      <description>Local scratch space for Hive jobs</description>
    </property>
    <property>
      <name>hive.downloaded.resources.dir</name>
      <value>/tmp/hive/${hive.session.id}_resources</value>
      <description>Temporary local directory for added resources in the remote file system.</description>
    </property>
    <property>
      <name>hive.querylog.location</name>
      <value>/tmp/hive/${user.name}</value>
      <description>Location of Hive run time structured log file</description>
    </property>
    <property>
      <name>hive.server2.logging.operation.log.location</name>
      <value>/tmp/hive/${user.name}/operation_logs</value>
      <description>Top level directory where operation logs are stored if logging functionality is >
    </property>
    <property>
      <name>hive.downloaded.resources.dir</name>
      <value>/tmp/hive/${hive.session.id}_resources</value>
      <description>Temporary local directory for added resources in the remote file system</description>
    </property>
~~~

5. To allow Hive to connect to Derby through JDBC, add the following lines to the file ~/hadoop3/etc/hadoop/core-site.xml of your Hadoop installation. Then restart your hadoop, i.e. DFS and YARN services. Note that your hive SHOULD NOT running at this step
 ~~~xml
    <configuration>
      <property>
       <name>hadoop.proxyuser.hduser.hosts</name>
       <value>*</value>
      </property>
      <property>
       <name>hadoop.proxyuser.hduser.groups</name>
       <value>*</value>
      </property>
    </configuration>
 ~~~


6. To allow PySpark to connect to hive cluster, duplicate (by overwriting) the following files to destination Spark
~~~bash
$ cp -f ~/hadoop3/etc/hadoop/core-site.xml ~/spark/conf/
$ cp -f ~/hadoop3/etc/hadoop/hdfs-site.xml ~/spark/conf/
$ cp -f ~/hive/conf/hive-site.xml ~/spark/conf/
$ cp -f ~/derby/lib/derbyclient.jar ~/spark/jars/
$ cp -f ~/derby/lib/derbytools.jar ~/spark/jars/
~~~
> Attention: ensure Hadoop, Spark, and Hive are successfully installed BEFORE proceeding with this step. You need to redo this step if ANY CHANGES on the involved files in nearly future.


7. Use existing session (named as **session 1**) to start DFS, YARN, Zookeeper, Kafka and HBase services before running the Derby as follows. Note that you need to press Enter key upon the execution of second of command. It will create databases in the current directory by default
~~~bash
$ cd ~/derby/data
$ nohup ~/derby/bin/startNetworkServer -h 0.0.0.0 &
$ jps
25410 NetworkServerControl
8711 ...
9561 ...
26699 Jps
9115 ...
8428 ...
~~~
> To stop Derby (replace 25410 with your process id):
> ~~~bash
> $ kill -9 25410
> ~~~
> Tips to start DFS, YARN, Zookeeper, Kafka, HBase and HappyBase (optional) services. Note that a total of elevan (11) services can be observed eventually, included HappyBase and Jps
> ~~~bash
> $ jps
> $ cd ~/hadoop3
> $ sbin/start-dfs.sh
> $ sbin/start-yarn.sh
> $ jps
> $ cd ~/kafka
> $ bin/zookeeper-server-start.sh config/zookeeper.properties &
> $ bin/kafka-server-start.sh config/server.properties &
> $ jps
> $ cd ~/hbase
> $ bin/start-hbase.sh
> $ jps
> $ bin/hbase thrift start -p 9090 &
> $ jps
> ~~~



8. Use the **session 1** to run the following command to initialize Derby as the Metastore database for Hive. It might take a few minutes. Please be patient while you wait for its completion
~~~bash
$ cd ~
$ java -cp /home/hduser/hive/lib/*:/home/hduser/spark/jars/*:/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/*:/home/hduser/hadoop3/share/hadoop/hdfs/lib/*:/home/hduser/hadoop3/share/hadoop/hdfs/*:/home/hduser/hadoop3/share/hadoop/tools/lib/*:/home/hduser/hadoop3/share/hadoop/mapreduce/*:/home/hduser/hadoop3/share/hadoop/yarn/*:/home/hduser/hadoop3/share/hadoop/yarn/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/*:/home/hduser/hadoop3/share/hadoop/yarn/timelineservice/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/lib/*:/home/hduser/hadoop3/share/hadoop/yarn/csi/* org.apache.hive.beeline.HiveSchemaTool -initSchema -dbType derby
~~~
> In the setup of Derby 10.14.2.0 with Hive 2.3.9, you need to stop-start the Derby after this step
> 
> Note that in the setup of Hive 3.3.1, you may have corrupted characters in file ~/hive/conf/hive-site.xml, which is inherited from its template (duplicated in Section M1), particularlly under the description of 'hive.txn.xlock.iow'. Just delete it if there is an error, and re-run above command


## M3. Running HiveServer2 and Beeline [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. HiveServer2 (HS2) is a service that enables clients to execute queries against Hive. HS2 supports multi-client concurrency and authentication. It is designed to provide better support for open API clients like JDBC and ODBC. Read more on HiveServer2 at URL https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Overview

2. HiveServer2 supports a command shell Beeline that works with HiveServer2. It's a JDBC client that is based on the SQLLine CLI (http://sqlline.sourceforge.net/). Read more on Beeline at URL https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients

3. Assume other neccessary services have been executed in the **session 1**.

4. Use another session (named as **session 2**) to edit the ~/hadoop3/etc/hadoop/hadoop-env.sh file by including additional paths into the HADOOP_CLASSPATH as follows
~~~bash
 export HADOOP_CLASSPATH=/usr/lib/jvm/java-8-openjdk-amd64/lib/tools.jar:/home/hduser/hive/lib/*:/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/* 
~~~
> Comment out the above line after stopping HiveServer2 to prevent HBase errors in the next round.

5. Switch back to shell **session 1**, and run the Hive over there. Leave the session running and DO NOT CLOSE the session after the execution of following command. 
~~~bash
$ cd ~/hive
$ bin/hiveserver2
~~~
> To stop the Hive, use Ctrl-C.

6. Switch back to shell **session 2**, and ensure an additional one service is observed using command jps, i.e. RunJar process before run Beeline shell. 
~~~bash
$ jps
$ cd ~/hive
$ bin/beeline
Beeline version 2.3.9 by Apache Hive
beeline> !connect jdbc:hive2://
Connecting to jdbc:hive2://
Enter username for jdbc:hive2://: APP
Enter password for jdbc:hive2://: mine
Connected to: Apache Hive (version 2.3.9)
Driver: Hive JDBC (version 2.3.9)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://>
~~~
> To stop the Beeline, use either command !q (after login) or Ctrl-c (before login).
> Note that we always need to terminate the running RunJar service/s BEFORE re-run the HiveServer2.

## M4. Quick start with Hive [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Create a new database
~~~bash
0: jdbc:hive2://> show databases;
0: jdbc:hive2://> create database choojunhive;
0: jdbc:hive2://> show databases;
~~~

2. Create a new table
~~~bash
0: jdbc:hive2://> CREATE TABLE IF NOT EXISTS choojunhive.employee (id int, name string, age int, gender string) COMMENT 'Employee Table' ROW FORMAT DELIMITED FIELDS TERMINATED BY ',';
0: jdbc:hive2://> DESCRIBE choojunhive.employee;
~~~

3. Insert and retrive records 
~~~bash
0: jdbc:hive2://> INSERT INTO choojunhive.employee VALUES (1,'Ahmad',19,'M');
0: jdbc:hive2://> INSERT INTO choojunhive.employee VALUES (2,'Baba',20,'M');
0: jdbc:hive2://> INSERT INTO choojunhive.employee VALUES (3,'Chong',21,'M');
0: jdbc:hive2://> select * from choojunhive.employee;
~~~

4. Try out the Guided Tutorials as follows
   * https://cwiki.apache.org/confluence/display/hive/tutorial
   * https://sparkbyexamples.com/apache-hive-tutorial/
   * https://www.simplilearn.com/tutorials/hadoop-tutorial/hive
