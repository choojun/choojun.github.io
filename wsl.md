# Microsoft Windows Subsystem for Linux (WSL)

## A. Remove/Setup WSL in Windows 10

1. Find Settings --> Apps --> Apps & features --> Programs and Features --> Turn Windows features on or off, or find Settings --> System -->  Optional features --> More Windows features --> Turn Windows features on or off.

2. Suppose that your PC has accessible to Internet (for huge files downloading) and Windows has updated to latest patches (e.g., version 22H2 for Windows 10).
Click (for setup) or unclick (for remove) the following items
    - Hyper-V
    - Virtual Machine Platform
    - Windows Hypervisor Platform
    - Windows Subsystem for Linux.

3. Restart PC

4. Suppose that your PC accessible to Internet (for huge files downloading), perform the search of required keyword, e.g., Ubuntu, in Microsoft Store. Click on the Get button to download and intall the required distro, e.g., UBuntu-xx.xx, and laumch the installed distro via the created link in the Windows Start. A sudoer user will be created during the initial launching.

![ubuntu](https://github.com/choojun/choojun.github.io/assets/6356054/d2a117c5-2471-4834-8cf2-d461ed3fbb5c)
> To set up the required Ubuntu distro (same version), run the following commands in PowerShell as administrator.
>
> ~~~
> wsl --update
> (restart PC)
>
> wsl –-list --online
>
> wsl --install -d UBuntu-xx.xx
> (restart PC)
> ~~~
>
> You may install a specific version of distro under the PowerShell as Administrator, e.g., the adopted version as PC either in lab or your course mate. 
>
> ![image](https://github.com/choojun/choojun.github.io/assets/6356054/e6259ec4-76fa-48a4-ab5f-0ba0b0b524c4)
>
> Locate your administrator username and password, after reboot and before first use.
>
> ![image](https://github.com/choojun/choojun.github.io/assets/6356054/7c24254b-a6c5-45a4-9973-6bace4969717)




## B. Running Ubuntu on WSL

1. Suppose that the required distro as default, run the following command with Power Shell.
~~~
wsl ~
~~~

2. Suppose that more than one distro setup in WSL, e.g. after importing backup distro, you may list them and run the particular distro (using created user account AND remember to change it home directory using command ‘cd  ~’) with commands as follows.
~~~
wsl –l -v
wsl –d <distro name> -u <created user in distro>
    e.g. wsl –d Ubuntu-22.04 –u tarumt
~~~

3. You may set the targeted distro as default with commands as follows.
~~~
wsl –-set-default (distro name)
    e.g. wsl –set-default Ubuntu-22.04
wsl –l -v
~~~

4. Even when issue ‘exit’ command, it does not turn off your WSL distro. You may terminate a running WSL distro with commands as follows.
~~~
wsl –t (distro name)
    e.g. wsl –t Ubuntu-22.04
~~~

5. Alternatively, you may issue the following command to terminate a running Linux, e.g. Ubuntu, as follows. Note that you need to provide a valid password (current user) when prompt. The WSL distro may terminate a few minutes later.
~~~
sudo shutdown –r 0
~~~





## C. Backup the Distro on WSL

1. In Power Shell, list the installed distro on WSL with the following command. Note that you need to know the exact name to create a backup
~~~
wsl –l -v
~~~

2. Change the directory that you want to save your backup with command ‘cd’. At the destination of directory, perform the following command to export the distribution of distro with Power Shell.
~~~
wsl --export (distribution) (filename.tar)
    e.g. wsl --export Ubuntu-22.04 my-backup-Ubuntu-22.04.tar
~~~

3. Alternatively, instead of using ‘cd’ to get to the destination directory, you may specify the file location and filename as part of export process in the Power Shell as follows.
~~~
wsl --export (distribution) (file location with filename.tar)
    e.g. wsl --export Ubuntu-22.04 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~




## D. Restore (Import) the Distro on WSL

1. In Power Shell, list the installed distro on WSL before removing the same / targeted instance. Note that you need to know the exact name for this process, especially those who wants to restore it at some point on the same PC or on those PC with the same name for distribution of distro. Note that the 'unregister' command will unregister the distribution from WSL and deletes the root filesystems.
~~~
wsl –l –v
wsl --unregister (targeted distribution)
~~~

2. Import the distribution of distro with Power Shell. Note that you may import the same distribution of distro into multiple install location, and this is good to facilitate your software development and testing purposes.
~~~
wsl --import (distribution) (*install location) (filename.tar)
    e.g. wsl --import Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~

3. In Power Shell, verify the distro has been imported correctly.
~~~
wsl –l -v
~~~

4. The beauty of using both export and import commands is that you can quickly and easily setup the same environment on multiple machines or setup multiple distros in the same machine. Your users and passwords will be retained, including anything you have installed using the package manager.



-----------------------------------------------------------

## E. Hadoop Installation and Configuration
 
> 1. Throughout our practical, we will assume that the Hadoop user name is **hduser**.
> 2. The first time that you launch your WSL Linux distro, you will be prompted to create a default user account. You may choose to either 
>    * name the default user account as hduser, or 
>    * create a separate user account named hduser.

### E1. Setup User Environment
1.	Create a new group named hadoop
~~~
$ sudo addgroup hadoop
~~~

2.	Create a new user account named hduser (if applicable)
Add the new user:
~~~
$ sudo adduser hduser
~~~

3. Grant the user sudo privileges
~~~
$ sudo usermod -aG sudo hduser
~~~

4.	Add hduser to the hadoop group
~~~
$ sudo usermod -a -G hadoop hduser
~~~

5.	Switch to the user account hduser (if applicable)
~~~
$ su - hduser
~~~

6.	If you want to go back to your original user session
~~~
$ exit
~~~

### E2. Setup Operating System Environment
1.	Reboot/terminate Ubuntu in WSL, and run the following commands from Ubuntu with user hduser

2. Check if ssh has been installed
~~~
$ service ssh status
~~~

3. If ssh has not been installed, install ssh
~~~
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install ssh
~~~

4. Install pdsh
~~~
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install pdsh
~~~

5. Install Python (if necessary)
~~~
$ python3 --version 
$ sudo apt install software-properties-common
$ sudo apt update
$ sudo apt install python3
$ sudo apt install python3-dev
$ sudo apt install python3-wheel
~~~

6. Check current python versions and symlinks
~~~
$ ls -l /usr/bin/python* 
~~~

7. Set environment variables by editing your ~/.bashrc file (for hduser):
~~~
$ nano ~/.bashrc
~~~

8. In ~/.bashrc file, set Python 3 as the default python version, add the following command to set Python 3 as the default python version:
~~~
alias python=python3
~~~

9. In ~/.bashrc file, add the following lines at the end of the file based on the python3.x from your installation:
~~~
export CPATH=/usr/include/python3.10m:$CPATH
export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH
~~~

10. Save the ~/.bashrc file.

11. Source the file:
~~~
$ source ~/.bashrc
~~~

12. Install pip (if necessary)
~~~
$ sudo apt update
$ sudo apt install python3-pip
$ pip3 --version
$ sudo pip3 install --upgrade pip
~~~~

13. Check which Java version is installed in the distro
~~~
$ java -version
~~~

14. Install targeted OpenJDK
~~~
$ sudo apt-get install openjdk-8-jdk
~~~
> After this, check if the correct version of Java is installed.





### E3. Setup SSH and PDSH
Run the following commands from Ubuntu with user hduser
> Secure Shell (SSH), also sometimes called Secure Socket Shell, is a protocol for securely accessing your site’s server over an unsecured network. In other words, it’s a way to safely log in to your server remotely using your preferred command-line interface.

1. Check if the SSH service is running
~~~
$ service ssh status
~~~
> WSL does not automatically start sshd.

2. Start SSH
~~~
$ sudo service ssh start
~~~
> WSL does not automatically start sshd.

3. Open the SSH port (if necessary)
~~~
$ sudo ufw allow ssh
~~~
> Ubuntu comes with a firewall configuration tool, known as UFW. If the firewall is enabled on your system, make sure to open the SSH port.

4. Reload SSH
~~~
$ sudo /etc/init.d/ssh reload
~~~

5. Modify pdsh’s default rcmd to ssh, by checking your pdsh default rcmd rsh:
~~~
$ pdsh -q -w localhost
~~~

6. Modify pdsh’s default rcmd to ssh, by adding the following command  to ~/.bashrc file (for hduser)
~~~
export PDSH_RCMD_TYPE=ssh
~~~

7. Run the following command to ensure the settings applied.
~~~
$ source ~/.bashrc
~~~



### E4. Disable IPv6
Run the following commands from Ubuntu with user hduser

1. Edit the /etc/sysctl.conf file with the following command.
~~~
$ sudo nano /etc/sysctl.conf
~~~

2. Add the following lines to the end of the sysctl.conf file:
~~~
# disable ipv6
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
~~~

3. To reload the /etc/sysctl.conf settings, issue the following command.
~~~
$ sudo sysctl -p
~~~

4. Check if IPv6 has been successfully disabled
~~~
$ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
~~~
> 0 means IPv6 is enabled; 1 means IPv6 is disabled.

5. Suppose that IPv6 is still enabled after rebooting, you must carry out the following:

   - Create (with root privileges) the file /etc/rc.local 
   ~~~
   $ sudo nano /etc/rc.local
   ~~~
   - Insert the following into the file /etc/rc.local:
   ~~~
   #!/bin/bash
   /etc/sysctl.d
   /etc/init.d/procps restart
   exit 0
   ~~~
   - Make the file executable
   ~~~
   sudo chmod 755 /etc/rc.local
   ~~~


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
 ~~~
<configuration>
   <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
 ~~~

9. Edit the etc/hadoop/mapred-site.xml file by adding the following configuration.
 ~~~
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
 ~~~
<configuration>
    <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
    </property>
    <property>
            <name>yarn.nodemanager.env-whitelist</name>    
<value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
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

## E8. Run a MapReduce Job in Hadoop
1. Ensure that you are login with the hduser 

2. Copy the input files into the distributed file system

~~~
$ hdfs dfs -mkdir input
$ hdfs dfs -put etc/hadoop/*.xml input
~~~

3. Run a MapReduce job from an example

~~~
$ bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar grep input output 'dfs[a-z.]+'
~~~

4. View the output files on the distributed system

~~~
$ hdfs dfs -cat output/*
~~~

> Sample output:
> Output:
> 1       dfsadmin
> 1       dfs.replication

5. Stop all the daemons

~~~
$ sbin/stop-yarn.sh
$ sbin/stop-dfs.sh
~~~

6. Logout from the hduser account and your tarumt account.
> At the beginning of all future practical, remember to carry out the following steps in Ubuntu:
> 1. Login as hduser or switch account to hduser
> ~~~
> $ su - hduser
> ~~~
> 2. Start ssh,
> ~~~
> $ sudo service ssh start
> ~~~
> 3. Start the HDFS service
> ~~~
> $ sbin/start-dfs.sh
> ~~~
> 4. Start the Yarn service
> ~~~
> $ sbin/start-yarn.sh
> ~~~
> At the end of the practical
> 1. Stop the YARN service
> ~~~
> $ sbin/stop-yarn.sh
> ~~~
> 2. Stop the HDFS service
> ~~~
> $ sbin/stop-dfs.sh
> ~~~
> 3. Exit from your user account(s) 
> ~~~
> exit
> ~~~
> 4. Issue command top to examine all expected services terminated


## F. Remove/Setup 
cc

### F1. Setup ...
### F2. Setup ...
### F3. Setup ...


## G. Remove/Setup 
cc

### G1. Setup ...
### G2. Setup ...
### G3. Setup ...

## H. Remove/Setup 
cc

### H1. Setup ...
### H2. Setup ...
### H3. Setup ..

## I. Remove/Setup 
cc

### I1. Setup ...
### I2. Setup ...
### I3. Setup ...

## J. Remove/Setup 
cc

### J1. Setup ...
### J2. Setup ...
### J3. Setup ...


> [!NOTE]  
> Highlights information that users should take into account, even when skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]  
> Crucial information necessary for users to succeed.

> [!WARNING]  
> Critical content demanding immediate user attention due to potential risks.

> [!CAUTION]
> Negative potential consequences of an action.

https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

