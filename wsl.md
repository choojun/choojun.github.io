![WSLHome](https://github.com/choojun/choojun.github.io/assets/6356054/213b5f1f-0c3b-4e6e-9bc4-13c1f8977ba9)
![wsl](https://github.com/choojun/choojun.github.io/assets/6356054/80aae18a-def8-453d-bb99-e6b629dcc5ae)

## Data Engineering with Distributed Environment for Data Analytics: An One-Stop of Setup and Configuration in the Microsoft Windows Subsystem for Linux (WSL)

Data engineering involves building systems to collect and utilize data. This data typically supports subsequent analysis and data science, often incorporating machine learning. Making data usable generally requires significant computational resources, storage, and processing. In essence, data engineering is the design and development of systems that allow individuals to gather and analyze raw data from diverse sources and formats. This website provides a one-stop setup and configuration for the system using Microsoft WSL. Hereby, the website has also incorporated a suite of distributed-ready open source software and guided exploration through selected application samples in the following sections.

### A. Remove/Setup WSL

1. Hardware requirement: recommended **16GB RAM (or more)** and **CPU virtualization enabled** with **constant internet access** during exercises.
Suppose that your Windows has updated to the latest patches too (tested Windows 10 Pro with version 22H2)!  
Software reqirement:
   - Find Settings --> Apps --> Apps & features --> Related Settings --> Programs and Features --> Turn Windows features on or off, **or**
   - Find Settings --> System -->  Optional features --> Related Settings --> More Windows features --> Turn Windows features on or off.

3. Click (for setup) or unclick (for remove) the following items (if any available)
    - Hyper-V
    - Virtual Machine Platform
    - Windows Hypervisor Platform
    - Windows Subsystem for Linux.

4. Restart PC

5. Suppose that your PC accessible to the Internet (for huge files downloading), perform the search of required keyword, e.g., Ubuntu, in the Microsoft Store. Click on the Get button to download and intall the required distro, e.g., UBuntu-xx.xx, and laumch the installed distro via the created link in the Windows Start. A sudoer user will be created during the initial launching.

> To set up the required Ubuntu distro (same version), run the following commands in PowerShell as administrator.
>
> ~~~bash
> wsl --update
> (restart PC)
>
> wsl –-list --online
>
> wsl --install -d UBuntu-xx.xx
> (restart PC)
> ~~~
>
> You may install a specific version of distro under the PowerShell as Administrator, e.g., the adopted version as PC either in lab or your course mate. Then, locate your administrator username and password, after reboot and before first use.
>
> ![wsl_setup](https://github.com/user-attachments/assets/fae87c59-097a-43f4-bea1-1febbaf5e16a)
>




### B. Running Ubuntu on WSL

1. Suppose that the required distro as default, run the following command with Power Shell.
~~~bash
wsl ~
~~~

2. Suppose that more than one distro setup in WSL, e.g. after importing backup distro, you may list them and run the particular distro (using created user account AND remember to change it home directory using command ‘cd  ~’) with commands as follows.
~~~bash
wsl –l -v
wsl –d <distro name> -u <created user in distro>
    e.g. wsl –d Ubuntu-22.04 –u tarumt
~~~
> You need to run the following command to update your WSL (mandatory) after performing the Windows Update, regardless Windows 10 or Windows 11
~~~bash
> wsl --update
~~~
> Note that you need to configure your Windows (on Domain, Public and Private Profiles) if you failed to update the WSL.
> ![wsl_firewall](https://github.com/user-attachments/assets/192025eb-3c2b-452d-8298-37134f9e8b73)


3. You may set the targeted distro as default with commands as follows.
~~~bash
wsl –-set-default <distro name>
    e.g. wsl –set-default Ubuntu-22.04
wsl –l -v
~~~

4. Even when issue ‘exit’ command, it does not turn off your WSL distro. You may terminate a running WSL distro with commands as follows.
~~~bash
wsl –t <distro name>
    e.g. wsl –t Ubuntu-22.04
~~~

5. Alternatively, you may issue the following command to terminate a running Linux, e.g. Ubuntu, as follows. Note that you need to provide a valid password (current user) when prompt. The WSL distro may terminate a few minutes later.
~~~bash
sudo shutdown now
~~~





### C. Backup the Distro on WSL

1. In Power Shell, list the installed distro on WSL with the following command. Note that you need to know the exact name to create a backup
~~~bash
wsl –l -v
~~~

2. Change the directory that you want to save your backup with command ‘cd’. At the destination of directory, perform the following command to export the distribution of distro with Power Shell.
~~~bash
wsl --export <distribution> <filename.tar>
    e.g. wsl --export Ubuntu-22.04 my-backup-Ubuntu-22.04.tar
~~~

3. Alternatively, instead of using ‘cd’ to get to the destination directory, you may specify the file location and filename as part of export process in the Power Shell as follows.
~~~bash
wsl --export <distribution> <file location with filename.tar>
    e.g. wsl --export Ubuntu-22.04 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~




### D. Restore (Import) the Distro on WSL

1. In Power Shell, list the installed distro on WSL before removing the same / targeted instance. Note that you need to know the exact name for this process, especially those who wants to restore it at some point on the same PC or on those PC with the same name for distribution of distro. Note that the 'unregister' command will unregister the distribution from WSL and deletes the root filesystems.
~~~bash
wsl –l –v
wsl --unregister <targeted distribution>
~~~

2. Import the distribution of distro with Power Shell. Note that you may import the same distribution of distro into multiple install location, and this is good to facilitate your software development and testing purposes.
~~~bash
wsl --import <distribution> <*install location> <filename.tar>
    e.g. wsl --import Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~

3. In Power Shell, verify the distro has been imported correctly.
~~~bash
wsl –l -v
~~~

4. The beauty of using both export and import commands is that you can quickly and easily setup the same environment on multiple machines or setup multiple distros in the same machine. Your users and passwords will be retained, including anything you have installed using the package manager.

### Note for Apple MacOS users
If you are using Apple machines (MacBooks and iMacs with Intel/Apple Silicon processor), you may need to install VMware Fusion Pro (Free for personal use - https://blogs.vmware.com/teamfusion/2024/05/fusion-pro-now-available-free-for-personal-use.html ) to virtualize Ubuntu 22.04.x. You can find the specific 22.04.x installer at https://ubuntu.com/download/alternative-downloads. After completing the Ubuntu setup, follow the same instructions in Sections E to Z to set up the required environment.

-----------------------------------------------------------



### E. [Hadoop Installation and Configuration](wsl_hadoop)
![hadoop](https://github.com/choojun/choojun.github.io/assets/6356054/0233d690-e03c-4699-a0e9-e9f932139de3)
--> Hadoop is referred as an acronym for High Availability Distributed Object Oriented Platform. It is an open source framework based on Java that manages the storage and processing of large amounts of data for applications. Hadoop uses distributed storage and parallel processing to handle big data and analytics jobs, breaking workloads down into smaller workloads that can be run at the same time. It is also calimed as a mature batch-processing platform for the petabyte scale environment.

### F. [HDFS File Operations and MapReduce](wsl_hdfs)
![hadoop](https://github.com/choojun/choojun.github.io/assets/6356054/0233d690-e03c-4699-a0e9-e9f932139de3)
--> HDFS is a Distributed File System (DFS) that provides high-throughput access to application data.

### G. [Spark, PySpark, Spark SQL and Jupyter Notebook](wsl_pyspark)
![spark](https://github.com/choojun/choojun.github.io/assets/6356054/ed7bd2cd-1ce6-43db-8ebd-18d56f351a43)
--> Spark is a multi-language engine for executing data engineering, data science, and machine learning on single-node machines or clusters. It is also known as a high-performance in-memory data-processing framework.

### H. [Spark and Machine Learning](wsl_pyspark_ml)
![spark](https://github.com/choojun/choojun.github.io/assets/6356054/ed7bd2cd-1ce6-43db-8ebd-18d56f351a43)

### I. [Spark and Visualization](wsl_pyspark_viz)
![spark](https://github.com/choojun/choojun.github.io/assets/6356054/ed7bd2cd-1ce6-43db-8ebd-18d56f351a43)

### J. [Kafka Installation and Configuration](wsl_kafka)
![kafka](https://github.com/choojun/choojun.github.io/assets/6356054/9e24ef13-f41f-4d8a-a731-889cdb9f76df)
--> It is an open-source distributed event streaming platform used by thousands of companies for high-performance data pipelines, streaming analytics, data integration, and mission-critical applications.

### K. [HBase Installation and Configuration](wsl_hbase)
![hbase](https://github.com/choojun/choojun.github.io/assets/6356054/bb45df8d-cfca-4cf2-abf1-9520ee683a4e)
--> It is an open-source, distributed, time-series, non-relational database modeled after Google's Bigtable: A Distributed Storage System for Structured Data. It is also known as an NoSQL key/value database on Hadoop. You need it when you require random, real-time access to your big data, including very large tables with billions of rows and millions of columns, running on clusters of commodity hardware and Hadoop (with HDFS).

### L. [HappyBase Installation and Configuration](wsl_happybase)
![happybase](https://github.com/choojun/choojun.github.io/assets/6356054/b4fa6fea-61a0-44e4-b62e-a97584cfe798)
--> It is a developer-friendly Python library to interact with Apache HBase.

Access the accumulated setup distro (release 1.0.0.20240405) at URL https://github.com/choojun/wsl/releases 

### M. [Hive Installation and Configuration](wsl_hive)
![hive](https://github.com/choojun/choojun.github.io/assets/6356054/18bc2a47-52bc-4e0c-a0c2-5c373b9db3d3)
--> It is a distributed, fault-tolerant data warehouse system enabling large-scale analytics. It facilitates reading, writing, and managing petabytes of data residing in distributed storage using SQL.

Access the accumulated setup distro (release 1.0.0.20240422) at URL https://github.com/choojun/wsl/releases 

-----------------------------------------------------------

### Y. Summary and Tested Version

| Requirement\Section |  [E](wsl_hadoop)  |  [F](wsl_hdfs) |  [G](wsl_pyspark) | [H](wsl_pyspark_ml)  |  [I](wsl_pyspark_viz)  |  [J](wsl_kafka)  |  [K](wsl_hbase)  |  [L](wsl_happybase)  |  [M](wsl_hive) |
|---------------------|:--------------------:|:--------------------:|:--------------------:|:--------------------:|:--------------------:|:--------------------:|:--------------------:|:--------------------:|:--------------------:|
| Hadoop              | 3.3.6 | *1 | *1                  | *1 | *1 | *1     | *1                   | *1    | *1        |
| Spark               |       |    | 3.5.1               | *1 | *1 |        | *1                   |       | *1        |
| Scala               |       |    | Targeted on 2.13.x  |    |    | 2.13.x |  Targeted on 2.13.x  |       |           |
| Jupyter Notebook    |       |    | 6.4.8               |    | *1 |        |                      |       |           |
| Kafka               |       |    |                     |    |    | 3.7.0  | *1                   | *1    |           |
| HBase               |       |    |                     |    |    |        | 2.5.7                | *1    |           |
| HappyBase           |       |    |                     |    |    |        |                      | 1.2.0 |           |
| Derby               |       |    |                     |    |    |        |                      |       | 10.14.2.0 |
| Hive                |       |    |                     |    |    |        |                      |       | 2.3.9     |
| Web Browser         | *1 |    | *1 |    | *1 |    |    |    |    |
| SSH and PDSH        | *1 | *1 | *1 | *1 | *1 | *1 | *1 | *1 | *1 |
| Internet            | *2 | *2 | *2 | *2 | *2 | *2 | *2 | *2 | *2 |
| WSL                 | *3 | *3 | *3 | *3 | *3 | *3 | *3 | *3 | *3 |

> *1 Needed
> 
> *2 Constant access
>
> *3 WSL Version 2 with distro Ubuntu 22.04

-----------------------------------------------------------

### Z. What's Next? (After setup above)
1. Start both DFS and YARN services. Ensure both websites http://localhost:9870/ and http://localhost:8088/ are up and ready. Six services can be observed using command jps in this step, i.e. Jps, DataNode, NameNode, SecondaryNameNode, NameManager and ResourceManager processes. Then, your WSL distro is ready for your daily practical exercises :D
~~~bash
      $ cd ~/hadoop3
      $ sbin/start-dfs.sh
      $ sbin/start-yarn.sh
~~~

2. Run PySpark Interactive Shell
~~~bash
      $ cd ~
      $ pyspark
~~~
3. Run jupyter notebook server after copying those required files from Windows (c:\de\sparkexercise) to Ubuntu (/home/hduser/sparkexercise) via the named directory sparkexercise. Then, copy and paste one of the URLs that are listed in any web browser
~~~bash
      $ sudo cp -r /mnt/c/de/sparkexercise /home/hduser
      $ sudo chown -r hduser:hduser -R /home/hduser/sparkexercise
      $ cd ~/sparkexercise
      $ jupyter notebook --port=8888 --no-browser
~~~
4. Run Kafka. An additional two services can be observed using command jps in this step, i.e. QuorumPeerMain (for Zookeeper) and Kafka processes
~~~bash
      $ cd ~/kafka
      $ bin/zookeeper-server-start.sh config/zookeeper.properties &
      $ bin/kafka-server-start.sh config/server.properties &
~~~
5. Run HBase. An additional two services can be observed using command jps in this step, i.e. HMaster and HRegionServer processes
~~~bash
      $ cd ~/hbase
      $ bin/start-hbase.sh
~~~
6. Run HBase shell.
~~~bash
      $ cd ~/hbase
      $ bin/hbase shell
~~~
7. Run HappyBase with Python. An additional one service can be observed using command jps in this step, i.e. ThriftServer process
~~~bash
      $ cd ~/hbase
      $ bin/hbase thrift start -p 9090 &
~~~
> Open another session in the terminal, and log in as hduser. Then, launch the Python command-line interpreter using the command python to explore the ability of HappyBase to interact with HBase using the Python programming language
8. Run Derby for Hive use. An additional one service can be observed using command jps in this step, i.e. NetworkServerControl process
~~~bash
      $ cd ~/derby/data
      $ nohup ~/derby/bin/startNetworkServer -h 0.0.0.0 &
~~~

9. Assume previous steps 1 to 8 are done in **session 1**.

10. Use another session (named as **session 2**) to edit the ~/hadoop3/etc/hadoop/hadoop-env.sh file by including additional paths into the HADOOP_CLASSPATH as follows
~~~bash
 export HADOOP_CLASSPATH=/usr/lib/jvm/java-8-openjdk-amd64/lib/tools.jar:/home/hduser/hive/lib/*:/home/hduser/hadoop3/share/hadoop/common/*:/home/hduser/hadoop3/share/hadoop/common/lib/*:/home/hduser/hadoop3/share/hadoop/client/* 
~~~
> Comment out the above line after stopping HiveServer2 to prevent HBase errors in the next round.

11. Switch back to shell **session 1**, and run the Hive over there. Leave the session running and DO NOT CLOSE the session after the execution of following command. 
~~~bash
$ cd ~/hive
$ bin/hiveserver2
~~~
> To stop the Hive, use Ctrl-C.

12. Switch back to shell **session 2**, and ensure an additional one service is observed using command jps, i.e. RunJar process before run Beeline shell. 
~~~bash
      $ jps
      $ cd ~/hive
      $ bin/beeline
~~~
> To stop the Beeline, use either command !q (after login) or Ctrl-c (before login).

![exclamation_mark](https://github.com/choojun/choojun.github.io/assets/6356054/dd0eeedb-feac-476d-a69c-6a0aa31d4159) Remember to stop the DFS, YARN, and other started services (in reverse order) to avoid data corruption in HDFS before shutting down your PC. Read the required details from sections E to M above. Tips to stop running processes:
1. Use Ctrl-c (before login) or !q (after login) for the **Beeline shell**
2. Use Ctrl-c for the RunJar of **HiveServer2** (at the running terminal)
3. Terminate the NetworkServerControl of **Derby** using its current process id (observed from command jps)
4. Terminate the ThriftServer of **HappyBase** using its current process id (observed from command jps)
5. Use command exit for **HBase shell**
6. Terminate the HMaster and HRegionServer of **HBase** using command
   > ~~~bash
   > $ cd ~/hbase
   > $ bin/stop-hbase.sh
   > ~~~
7. Terminate the QuorumPeerMain (from **Zookeeper**) and **Kafka** using command
   > ~~~bash
   > $ cd ~/kafka
   > $ bin/kafka-server-stop.sh
   > $ bin/zookeeper-server-stop.sh
   > ~~~
8. Use command exit() for **PySpark interactive shell**
9. Terminate the NodeManager, ResourceManager (from **YARN**) and DataNode, NameNode, SecondaryNameNode (from **DFS**) using command
   > ~~~bash
   > $ cd ~/hadoop3
   > $ sbin/stop-yarn.sh
   > $ sbin/stop-dfs.sh
   > ~~~
10. Use command jps to check remaining processes. Terminate each of them with its current process id, if any (observed from command jps)

### References

1. https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

2. https://en.wikipedia.org/wiki/Apache_Hadoop

3. https://hadoop.apache.org/

4. https://hbase.apache.org/book.html#hadoop

5. https://en.wikipedia.org/wiki/List_of_POSIX_commands

6. https://hadoop.apache.org/docs/r3.3.6/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html

7. https://en.wikipedia.org/wiki/MapReduce

8. https://spark.apache.org

9. https://en.wikipedia.org/wiki/Apache_Spark

10. https://spark.apache.org/sql/

11. https://en.wikipedia.org/wiki/Apache_Spark#Spark_SQL

12. https://pypi.org/project/reverse_geocoder/

13. https://jupyter.org

14. https://en.wikipedia.org/wiki/Project_Jupyter

15. https://spark.apache.org/docs/latest/sql-programming-guide.html

16. https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds

17. https://spark.apache.org/docs/latest/sql-programming-guide.html

18. https://spark.apache.org/docs/latest/ml-guide.html#announcement-dataframe-based-api-is-primary-api

19. https://seaborn.pydata.org/

20. https://kafka.apache.org/documentation/

21. https://kafka.apache.org/quickstart

22. https://developer.twitter.com/en/apply-for-access

23. https://en.wikipedia.org/wiki/Berkeley_DB

24. https://hbase.apache.org/book.html

25. https://hbase.apache.org/book.html#hadoop

26. https://happybase.readthedocs.io/en/latest/

27. https://happybase.readthedocs.io/en/latest/user.html

28. https://cwiki.apache.org/confluence/display/Hive/GettingStarted

29. https://cwiki.apache.org/confluence/display/Hive/Home

30. https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Overview

31. http://sqlline.sourceforge.net/

32. https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients

33. https://cwiki.apache.org/confluence/display/hive/tutorial

34. https://sparkbyexamples.com/apache-hive-tutorial/

35. https://www.simplilearn.com/tutorials/hadoop-tutorial/hive

36. https://cwiki.apache.org/confluence/display/hive/tutorial

37. https://sparkbyexamples.com/apache-hive-tutorial/

38. https://www.simplilearn.com/tutorials/hadoop-tutorial/hive

