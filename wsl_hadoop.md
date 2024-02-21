# E. Hadoop Installation and Configuration
 
> 1. Throughout our practical, we will assume that the Hadoop user name is **hduser**.
> 2. The first time that you launch your WSL Linux distro, you will be prompted to create a default user account. You may choose to either 
>    * name the default user account as tarumt, and 
>    * create a separate user account named hduser.
> 3. The NameNode is the hardware that contains the GNU/Linux operating system and software. The Hadoop distributed file system acts as the master server and can manage the files, control a client's access to files, and overseas file operating processes such as renaming, opening, and closing files. A DataNode is hardware having the GNU/Linux operating system and DataNode software. For every node in a HDFS cluster, you will locate a DataNode. These nodes help to control the data storage of their system as they can perform operations on the file systems if the client requests, and also create, replicate, and block files when the NameNode instructs.   ![hdfsarchitecture-image](https://github.com/choojun/choojun.github.io/assets/6356054/347b07e8-385f-416d-9371-e43407251f74)
> 4. The HDFS file system consists of a set of Master services (NameNode, secondary NameNode, and DataNodes). The NameNode and secondary NameNode manage the HDFS metadata. The DataNodes host the underlying HDFS data. The NameNode tracks which DataNodes contain the contents of a given file in HDFS. HDFS divides files into blocks and stores each block on a DataNode. Multiple DataNodes are linked to the cluster. The NameNode then distributes replicas of these data blocks across the cluster. It also instructs the user or application where to locate wanted information.
> 5. Read more on Hadoop at URL https://en.wikipedia.org/wiki/Apache_Hadoop

## E1. Setup User Environment
1.	Create a new group named hadoop
~~~bash
$ sudo addgroup hadoop
~~~

2.	Create a new user account named hduser (if applicable)
Add the new user:
~~~bash
$ sudo adduser hduser
~~~

3. Grant the user sudo privileges
~~~bash
$ sudo usermod -aG sudo hduser
~~~

4.	Add hduser to the hadoop group
~~~bash
$ sudo usermod -a -G hadoop hduser
~~~

5.	Switch to the user account hduser (if applicable)
~~~bash
$ su - hduser
~~~

6.	If you want to go back to your original user session
~~~bash
$ exit
~~~

## E2. Setup Operating System Environment
1.	Reboot/terminate Ubuntu in WSL, and run the following commands from Ubuntu with user hduser

2. Check if ssh has been installed
~~~bash
$ service ssh status
~~~

3. If ssh has not been installed, install ssh
~~~bash
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install ssh
~~~

4. Install pdsh
~~~bash
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install pdsh
~~~

5. Install Python (if necessary)
~~~bash
$ python3 --version 
$ sudo apt install software-properties-common
$ sudo apt update
$ sudo apt install python3
$ sudo apt install python3-dev
$ sudo apt install python3-wheel
~~~

6. Check current python versions and symlinks
~~~bash
$ ls -l /usr/bin/python* 
~~~

7. Set environment variables by editing your ~/.bashrc file (for hduser):
~~~bash
$ nano ~/.bashrc
~~~

8. In ~/.bashrc file, set Python 3 as the default python version, add the following command to set Python 3 as the default python version:
~~~bash
alias python=python3
~~~

9. In ~/.bashrc file, add the following lines at the end of the file based on the python3.x from your installation:
~~~bash
export CPATH=/usr/include/python3.10m:$CPATH
export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH
~~~

10. Save the ~/.bashrc file.

11. Source the file:
~~~bash
$ source ~/.bashrc
~~~

12. Install pip (if necessary)
~~~bash
$ sudo apt update
$ sudo apt install python3-pip
$ pip3 --version
$ sudo pip3 install --upgrade pip
~~~~

13. Check which Java version is installed in the distro
~~~bash
$ java -version
~~~

14. Install targeted OpenJDK
~~~bash
$ sudo apt-get install openjdk-8-jdk
~~~
> After this, check if the correct version of Java is installed.





## E3. Setup SSH and PDSH
Run the following commands from Ubuntu with user hduser
> Secure Shell (SSH), also sometimes called Secure Socket Shell, is a protocol for securely accessing your site’s server over an unsecured network. In other words, it’s a way to safely log in to your server remotely using your preferred command-line interface.

1. Check if the SSH service is running
~~~bash
$ service ssh status
~~~
> WSL does not automatically start sshd.

2. Start SSH
~~~bash
$ sudo service ssh start
~~~
> WSL does not automatically start sshd.

3. Open the SSH port (if necessary)
~~~bash
$ sudo ufw allow ssh
~~~
> Ubuntu comes with a firewall configuration tool, known as UFW. If the firewall is enabled on your system, make sure to open the SSH port.

4. Reload SSH
~~~bash
$ sudo /etc/init.d/ssh reload
~~~

5. Modify pdsh’s default rcmd to ssh, by checking your pdsh default rcmd rsh:
~~~bash
$ pdsh -q -w localhost
~~~

6. Modify pdsh’s default rcmd to ssh, by adding the following command  to ~/.bashrc file (for hduser)
~~~bash
export PDSH_RCMD_TYPE=ssh
~~~

7. Run the following command to ensure the settings applied.
~~~bash
$ source ~/.bashrc
~~~



## E4. Disable IPv6
Run the following commands from Ubuntu with user hduser

1. Edit the /etc/sysctl.conf file with the following command.
~~~bash
$ sudo nano /etc/sysctl.conf
~~~

2. Add the following lines to the end of the sysctl.conf file:
~~~bash
# disable ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
~~~

3. To reload the /etc/sysctl.conf settings, issue the following command.
~~~bash
$ sudo sysctl -p
~~~

4. Check if IPv6 has been successfully disabled
~~~bash
$ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
~~~
> 0 means IPv6 is enabled; 1 means IPv6 is disabled.

5. Suppose that IPv6 is still enabled after rebooting, you must carry out the following:

   - Create (with root privileges) the file /etc/rc.local 
   ~~~bash
   $ sudo nano /etc/rc.local
   ~~~
   - Insert the following into the file /etc/rc.local:
   ~~~bash
   #!/bin/bash
   /etc/sysctl.d
   /etc/init.d/procps restart
   exit 0
   ~~~
   - Make the file executable
   ~~~bash
   sudo chmod 755 /etc/rc.local
   ~~~


## E5. Install Hadoop
1. Switch to the hduser account

2. Download the Hadoop binary by finding the appropriate Hadoop binary from the Hadoop releases page.
> We will use Hadoop 3.3.6 to avoid problems with HBase in a later practical. Read more at URL https://hbase.apache.org/book.html#hadoop
~~~bash
$ wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
~~~
> Or, you may copy the downloaded tar.gz file manually to destination ~/hduser/

3.	Untar the file
~~~bash
$ tar -xvzf hadoop-3.3.6.tar.gz
~~~

4.	Rename the folder as hadoop3
~~~bash
$ mv hadoop-3.3.6 hadoop3
$ sudo chown -R hduser:hadoop hadoop3
$ sudo chmod g+w -R hadoop3
~~~

## E6. Configure passphraseless ssh for Hadoop
1. Ensure that you are login with the hduser 

2. Ensure that you can SSH to the localhost in Ubuntu

3. To ssh to localhost without a passphrase, run the following command to initialize your private and public keys:
~~~bash
$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys
~~~

4. Test the configuration
~~~bash
$ ssh localhost
~~~
> Exit the ssh by issuing command exit.

## E7. Configure Pseudo-distributed Mode for Hadoop
1. Ensure that you are login with the hduser 

2. Setup the environment variables in the ~/.bashrc file (for hduser) by adding the environment variables to the end of the file
 ~~~bash
 export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
 export HADOOP_HOME=/home/hduser/hadoop3
 export PATH=$PATH:$HADOOP_HOME/bin
 ~~~

3. Source the file:
 ~~~bash
 $ source ~/.bashrc
 ~~~

4. Change directory to the Hadoop folder
 ~~~bash
 $ cd hadoop3
 ~~~

5. Edit the etc/hadoop/hadoop-env.sh file by uncomment and set the environment variables as follows.
 ~~~bash
 export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
 export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar
 export HADOOP_LOG_DIR=${HADOOP_HOME}/logs
 ~~~

6. Verify if the installation was successful
 ~~~bash
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
 ~~~bash
 $ bin/hdfs namenode -format
 ~~~
 > Running the above command should display information on formating the namenode of Hadoop with installed Java. You should observe the namenode formated and terminated without any error. 

12. Start the Distributed File System (DFS) service
 ~~~bash
 $ sbin/start-dfs.sh
 ~~~
 > Start the NameNode and DataNode daemons
 ~~~bash
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
 ~~~bash
 $ sbin/start-yarn.sh
 ~~~
 > Start the Yarn daemon
 ~~~bash
 $ jps
 ~~~
 > Check the status. If the Yarn services are initiated successfully, you should see six processes.
 > ~~~
 > ResourceManager
 > NodeManager
 > ~~~
 > Browse any web browser for the ResourceManager, by default, and it is available at URL http://localhost:8088/

14. Create HDFS directories required to execute MapReduce jobs
 ~~~bash
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
$ jps
~~~
> Suppose you need to observe at least four (4) services, including both NameNode and DataNode, as stated in step 12 of G7. Otherwise, you may need to reformat the HDFS NameNode. Wait for the format process to complete without errors before proceeding. Note that formatting the NameNode will result in loss of all data files in HDFS. You also need to recreate the directory /user and its sub-directory /user/hduser after starting the HDFS service and before moving on to the next steps.
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
