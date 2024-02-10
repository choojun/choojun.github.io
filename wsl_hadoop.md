
## E5. Install Hadoop
1. Switch to the hduser account

2. Download the Hadoop binary by finding the appropriate Hadoop binary from the Hadoop releases page.
> We will use Hadoop 3.3.6 to avoid problems with HBase in a later practical. Read more at URL https://hbase.apache.org/book.html#hadoop
~~~
$ wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
~~~
> Or, you may copy the downloaded tar.gz file manually to destination ~/hduser/

3.	Untar the file
~~~
$ tar -xvzf hadoop-3.3.6.tar.gz
~~~

4.	Rename the folder as hadoop3
~~~
$ mv hadoop-3.3.6 hadoop3
$ sudo chown -R hduser:hadoop hadoop3
$ sudo chmod g+w -R hadoop3
~~~

## E6. Configure passphraseless ssh for Hadoop
1. Ensure that you are login with the hduser 

2. Ensure that you can SSH to the localhost in Ubuntu

3. To ssh to localhost without a passphrase, run the following command to initialize your private and public keys:
~~~
$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys
~~~

4. Test the configuration
~~~
$ ssh localhost
~~~
> Exit the ssh by issuing command exit.

## E7. Configure Pseudo-distributed Mode for Hadoop
1. Ensure that you are login with the hduser 

2. Setup the environment variables in the ~/.bashrc file (for hduser) by adding the environment variables to the end of the file
 ~~~
 export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
 export HADOOP_HOME=/home/hduser/hadoop3
 export PATH=$PATH:$HADOOP_HOME/bin
 ~~~

3. Source the file:
 ~~~
 $ source ~/.bashrc
 ~~~

4. Change directory to the Hadoop folder
 ~~~
 $ cd hadoop3
 ~~~

5. Edit the etc/hadoop/hadoop-env.sh file by uncomment and set the environment variables as follows.
 ~~~
 export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
 export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
 export HADOOP_LOG_DIR=${HADOOP_HOME}/logs
 ~~~

6. Verify if the installation was successful
 ~~~
 $ hadoop
 ~~~
 > Running the above command should display various options.

7. Edit the etc/hadoop/core-site.xml file by adding the following configuration.
 ~~~xml
 <configuration>
   <property>
     <name>fs.defaultFS</name>
     <value>hdfs://localhost:9000</value>
   </property>
 </configuration>
 ~~~

8. Edit the etc/hadoop/hdfs-site.xml file by adding the following configuration.
 ~~~xml
 <configuration>
     <property>
       <name>dfs.replication</name>
       <value>1</value>
     </property>
 </configuration>
 ~~~

9. Edit the etc/hadoop/mapred-site.xml file by adding the following configuration.
 ~~~xml
 <configuration>
   <property>
     <name>mapreduce.framework.name</name>
     <value>yarn</value>
   </property>
   <property>  
     <name>mapreduce.application.classpath</name>
     <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
    </property>
  </configuration>
 ~~~

10. Edit the etc/hadoop/yarn-site.xml file by adding the following configuration.
 ~~~xml
 <configuration>
   <property>
     <name>yarn.nodemanager.aux-services</name>
     <value>mapreduce_shuffle</value>
   </property>
   <property>
     <name>yarn.nodemanager.env-whitelist</name>    
     <value>JAVA_HOME, HADOOP_COMMON_HOME, HADOOP_HDFS_HOME, HADOOP_CONF_DIR, CLASSPATH_PREPEND_DISTCACHE, HADOOP_YARN_HOME, HADOOP_MAPRED_HOME</value>
   </property>
   <property>
     <name>yarn.resourcemanager.address</name>
     <value>127.0.0.1:8032</value>
   </property>
 </configuration>
 ~~~

11. Format the namenode 
 ~~~
 $ bin/hdfs namenode -format
 ~~~
 > Running the above command should display information on formating the namenode of Hadoop with installed Java. You should observe the namenode formated and terminated without any error. 
> 
12. Start the Distributed File System (DFS) service
 ~~~
 $ sbin/start-dfs.sh
 ~~~
 > Start the NameNode and DataNode daemons
 ~~~
 $ jps
 ~~~
 > Check the status. If the NameNode and DataNode services are initiated successfully, you should see these four processes
 > ~~~
 > DataNode
 > SecondaryNameNode
 > Jps
 > NameNode
 > ~~~
 > Browse any web browser for the NameNode, by default, and it is available at URL http://localhost:9870/

13. Start the Yarn service
 ~~~
 $ sbin/start-yarn.sh
 ~~~
 > Start the Yarn daemon
 ~~~
 $ jps
 ~~~
 > Check the status. If the Yarn services are initiated successfully, you should see six processes.
 > ~~~
 > ResourceManager
 > NodeManager
 > ~~~
 > Browse any web browser for the ResourceManager, by default, and it is available at URL http://localhost:8080/

14. Create HDFS directories required to execute MapReduce jobs
 ~~~
 $ hdfs dfs -mkdir /user
 $ hdfs dfs -mkdir /user/hduser
 ~~~

## E8. Run a sample MapReduce Job in Hadoop
1. Ensure that you are login with the hduser 

2. Copy the input files into the distributed file system
~~~bash
$ hdfs dfs -mkdir input
$ hdfs dfs -put etc/hadoop/*.xml input
~~~

3. Run a MapReduce job from an example
~~~bash
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep input output 'dfs[a-z.]+'
~~~

4. View the output files on the distributed system
~~~bash
$ hdfs dfs -cat output/*
~~~
> Output:
> 1       dfsadmin
> 1       dfs.replication

5. Stop all the daemons
~~~bash
$ sbin/stop-yarn.sh
$ sbin/stop-dfs.sh
~~~

6. Logout from the hduser account and your tarumt account.

## E9. Attention: At the beginning of all future practical
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
~~~

4. Start the Yarn service
~~~bash
$ sbin/start-yarn.sh
~~~

## E10. Attention: At the end of all future practical
1. Stop the YARN service
~~~bash
$ sbin/stop-yarn.sh
~~~

2. Stop the HDFS service
~~~bash
$ sbin/stop-dfs.sh
~~~

3. Exit from your user account(s) 
~~~bash
exit
~~~

4. Issue command top to examine all expected services terminated
