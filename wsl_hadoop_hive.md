![hive](https://github.com/choojun/choojun.github.io/assets/6356054/18bc2a47-52bc-4e0c-a0c2-5c373b9db3d3)

# M. Hive Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. It is a distributed, fault-tolerant data warehouse system that enables analytics at a massive scale and facilitates reading, writing, and managing petabytes of data residing in distributed storage using SQL
2. Read more on Hive at URL https://cwiki.apache.org/confluence/display/Hive/GettingStarted and https://cwiki.apache.org/confluence/display/Hive/Home

## M1. Install and Configure Hive [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser. Ensure that the SSH, DFS, and YARN services are started sequentially. Then, examine them using the jps command

2. Install Hive
~~~bash
$ cd ~
$ wget  https://dlcdn.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
$ tar -xvzf apache-hive-3.1.3-bin.tar.gz
$ mv apache-hive-3.1.3-bin hive
~~~

3. Edit the file ~/.bashrc with the the following lines 
~~~bash
 export HADOOP_MAPRED_HOME=/home/hduser/hadoop3
 export HADOOP_COMMON_HOME=/home/hduser/hadoop3
 export HADOOP_HDFS_HOME=/home/hduser/hadoop3
 export YARN_HOME=/home/hduser/hadoop3
 export HIVE_HOME=/home/hduser/hive
 export HIVE_CONF_DIR=/home/hduser/hive/conf
 export PATH=$PATH:/home/hduser/hive/bin
 export CLASSPATH=$CLASSPATH:/home/hduser/hive/lib/*:/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/*
 export HADOOP_CLASSPATH=$CLASSPATH
~~~

4. Re-load the environment
~~~bash
$ source ~/.bashrc
~~~

5. Duplicate for the file $HIVE_HOME/conf/hive-site.xml 
~~~bash
$ cd $HIVE_HOME
$ cp conf/hive-default.xml.template conf/hive-site.xml
~~~

6. Edit the file $HIVE_HOME/conf/hive-site.xml
* Replace all occurrences of ${system:java.io.tmpdir} to /tmp/hive and this location is the new location for Hive storing all its temporary files.
* Replace all occurrences of ${system:user.name} to ${user.name} which should be the user name you log in with
* Replace all occurrences of /user/hive tp /user/hduser
> You may want to use the nano's search (Ctrl-w with Enter) to find the targeted keyword, and replace them one by one.

7. Edit the file $HIVE_HOME/conf/hive-config.sh with the the following lines 
~~~bash
 export HADOOP_HOME=/home/hduser/hadoop3
~~~

8. Duplicate for the file $HIVE_HOME/conf/hive-env.sh 
~~~bash
$ cp $HIVE_HOME/conf/hive-env.sh.template $HIVE_HOME/conf/hive-env.sh
~~~

9. Edit the file $HIVE_HOME/conf/hive-env.sh  with the the following lines 
~~~bash
 export HADOOP_HOME=/home/hduser/hadoop3
 export HIVE_CONF_DIR=/home/hduser/hive/conf
~~~

10. Create Hive directories in the HDFS
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

11. Change mode of access for hadoop's data and namenode, which are created in Section E
~~~bash
$ chmod -R g+w /home/hduser/hadoopData
$ chmod -R g+w /home/hduser/hadoopName
~~~


12. Delete the log4j-slf4j-impl-2.17.1.jar file (optional)
~~~bash
$ rm $HIVE_HOME/lib/log4j-slf4j-impl-2.17.1.jar
~~~
> We delete the file log4j-slf4j-impl-2.17.1.jar because the similar file is also presented in the Hadoop directory, and it gives error to us occasionally


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

4. Configure the Hive to use Local/Network Derby by changing the following property values in file $HIVE_HOME/conf/hive-site.xml
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
      <value>/tmp/${user.name}</value>
      <description>Local scratch space for Hive jobs</description>
    </property>
    <property>
      <name>hive.downloaded.resources.dir</name>
      <value>/tmp/${hive.session.id}_resources</value>
      <description>Temporary local directory for added resources in the remote file system.</description>
    </property>
    <property>
      <name>hive.querylog.location</name>
      <value>/tmp/${user.name}</value>
      <description>Location of Hive run time structured log file</description>
    </property>
    <property>
      <name>hive.server2.logging.operation.log.location</name>
      <value>/tmp/${user.name}/operation_logs</value>
      <description>Top level directory where operation logs are stored if logging functionality is >
    </property>
    <property>
      <name>hive.downloaded.resources.dir</name>
      <value>/tmp/${user.name}_resources</value>
      <description>Temporary local directory for added resources in the remote file system</description>
    </property>
~~~

5. To allow Hive to connect to Derby through JDBC, add the following lines to the core-site.xml file located at $HADOOP_HOME/etc/hadoop/core-site.xml of your Hadoop installation. Then restart your hadoop, i.e. DFS and YARN services. Note that your hive SHOULD NOT running at this step
 ~~~xml
    <configuration>
      <property>
       <name>hadoop.proxyuser.hadoop.hosts</name>
       <value>*</value>
      </property>
      <property>
       <name>hadoop.proxyuser.hadoop.groups</name>
       <value>*</value>
      </property>
    </configuration>
 ~~~
> Tips to restart DFS and YARN services
> ~~~bash
> $ jps
> $ cd ~/hadoop3
> $ sbin/stop-yarn.sh
> $ sbin/stop-dfs.sh
> $ jps
> $ sbin/start-dfs.sh
> $ sbin/start-yarn.sh
> $ jps
> ~~~

6. To allow PySpark to connect to hive cluster, duplicate (by overwriting) the following files to destination Spark
~~~bash
$ cp -f $HADOOP_HOME/etc/hadoop/core-site.xml $SPARK_HOME/conf/
$ cp -f $HADOOP_HOME/etc/hadoop/hdfs-site.xml $SPARK_HOME/conf/
$ cp -f $HIVE_HOME/conf/hive-site.xml $SPARK_HOME/conf/
$ cp -f $DERBY_HOME/lib/derbyclient.jar $SPARK_HOME/jars/
$ cp -f $DERBY_HOME/lib/derbytools.jar $SPARK_HOME/jars/
~~~
> Ensure Hadoop, Spark, and Hive are successfully installed BEFORE proceeding with this step.
> You need to redo this step if any changes in the involved files in nearly future.


7. Run the Derby. Note that it will create databases in the current directory by default
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
~~~bash
$ kill -9 25410
~~~


8. Run the following command to initialize Derby as the Metastore database for Hive. It might take a few minutes. Please be patient while you wait for its completion
~~~bash
$ cp ~
$ java -cp /home/hduser/hive/lib/*:/home/hduser/hive/lib/*:/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/* org.apache.hive.beeline.HiveSchemaTool -initSchema -dbType derby
$
~~~
> You may have corrupted characters in file $HIVE_HOME/conf/hive-site.xml, which is inherited from its template (duplicated in Section M1), particularlly under the description of 'hive.txn.xlock.iow'. Just delete it if there is an error, and re-run above command


## M3. Running HiveServer2 and Beeline [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. HiveServer2 (HS2) is a service that enables clients to execute queries against Hive. HS2 supports multi-client concurrency and authentication. It is designed to provide better support for open API clients like JDBC and ODBC. Read more on HiveServer2 at URL https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Overview

2. HiveServer2 supports a command shell Beeline that works with HiveServer2. It's a JDBC client that is based on the SQLLine CLI (http://sqlline.sourceforge.net/). Read more on Beeline at URL https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients

3. Edit the $HADOOP_HOME/etc/hadoop/hadoop-env.sh file by including additional paths into the HADOOP_CLASSPATH as follows
~~~bash
 export HADOOP_CLASSPATH=/usr/lib/jvm/java-8-openjdk-amd64/lib/tools.jar:/home/hduser/hive/lib/*:/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/* 
~~~

4. Run HiveServer2 from shell
~~~bash
$ cd ~
$ /home/hduser/hive/bin/hiveserver2
~~~
> You may observe addition service has activated namely as RunJar, if it gives error to us
> Ctrl-c to terminate the RunJar service of HiveServer2

5. Run Beeline from shell with following inputs
~~~bash
$ cd ~
$ $HIVE_HOME/bin/beeline
Beeline version 3.1.3 by Apache Hive
beeline> !connect jdbc:hive2://
Connecting to jdbc:hive2://
Enter username for jdbc:hive2://: APP
Enter password for jdbc:hive2://: mine
Connected to: Apache Hive (version 3.1.3)
Driver: Hive JDBC (version 3.1.3)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://>
~~~
> To quit from the command shell Beeline
> ~~~bash
> !q
> ~~~~

## M4. Try out the Guided Tutorials on Hive [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. https://cwiki.apache.org/confluence/display/hive/tutorial
2. https://sparkbyexamples.com/apache-hive-tutorial/
3. https://www.simplilearn.com/tutorials/hadoop-tutorial/hive
