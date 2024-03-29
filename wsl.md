![WSLHome](https://github.com/choojun/choojun.github.io/assets/6356054/213b5f1f-0c3b-4e6e-9bc4-13c1f8977ba9)
![wsl](https://github.com/choojun/choojun.github.io/assets/6356054/80aae18a-def8-453d-bb99-e6b629dcc5ae)

# Microsoft Windows Subsystem for Linux (WSL)

## A. Remove/Setup WSL (tested on Windows 10 and WSL v2)

1. Hardware requirement: recommended **16GB RAM (or more)** and **CPU virtualization enabled** with **constant internet access** during exercises.
Suppose that your Windows has updated to the latest patches (e.g., version 22H2 and later for Windows 10) too!  
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
![ubuntu](https://github.com/choojun/choojun.github.io/assets/6356054/d2a117c5-2471-4834-8cf2-d461ed3fbb5c)
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
> You may install a specific version of distro under the PowerShell as Administrator, e.g., the adopted version as PC either in lab or your course mate. 
>
> ![image](https://github.com/choojun/choojun.github.io/assets/6356054/e6259ec4-76fa-48a4-ab5f-0ba0b0b524c4)
>
> Locate your administrator username and password, after reboot and before first use.
>
> ![image](https://github.com/choojun/choojun.github.io/assets/6356054/7c24254b-a6c5-45a4-9973-6bace4969717)




## B. Running Ubuntu on WSL

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
> You need to run the following command to update your WSL after performing the Windows Update, regardless Windows 10 or Windows 11
~~~bash
> wsl --update
~~~

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





## C. Backup the Distro on WSL

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




## D. Restore (Import) the Distro on WSL

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


-----------------------------------------------------------



## E. [Hadoop Installation and Configuration](wsl_hadoop)
![hadoop](https://github.com/choojun/choojun.github.io/assets/6356054/0233d690-e03c-4699-a0e9-e9f932139de3)

## F. [HDFS File Operations and MapReduce](wsl_hdfs)
![hadoop](https://github.com/choojun/choojun.github.io/assets/6356054/0233d690-e03c-4699-a0e9-e9f932139de3)

## G. [Spark, PySpark, Spark SQL and Jupyter Notebook](wsl_pyspark)
![spark](https://github.com/choojun/choojun.github.io/assets/6356054/ed7bd2cd-1ce6-43db-8ebd-18d56f351a43)

## H. [Spark and Machine Learning](wsl_pyspark_ml)
![spark](https://github.com/choojun/choojun.github.io/assets/6356054/ed7bd2cd-1ce6-43db-8ebd-18d56f351a43)

## I. [Spark and Visualization](wsl_pyspark_viz)
![spark](https://github.com/choojun/choojun.github.io/assets/6356054/ed7bd2cd-1ce6-43db-8ebd-18d56f351a43)

## J. [Kafka Installation and Configuration](wsl_hadoop_kafka)
![kafka](https://github.com/choojun/choojun.github.io/assets/6356054/9e24ef13-f41f-4d8a-a731-889cdb9f76df)

## K. [HBase Installation and Configuration](wsl_hadoop_hbase)
![hbase](https://github.com/choojun/choojun.github.io/assets/6356054/bb45df8d-cfca-4cf2-abf1-9520ee683a4e)

## L. [HappyBase Installation and Configuration](wsl_hadoop_happybase)
![happybase](https://github.com/choojun/choojun.github.io/assets/6356054/b4fa6fea-61a0-44e4-b62e-a97584cfe798)

## M. [Hive Installation and Configuration](wsl_hadoop_hive)
![hive](https://github.com/choojun/choojun.github.io/assets/6356054/18bc2a47-52bc-4e0c-a0c2-5c373b9db3d3)

-----------------------------------------------------------

## Z. What's Next? (After done the setup for above items)
Suppose that both DFS and YARN services running, by ensuring both websites http://localhost:9870/ and http://localhost:8088/ are up and ready. Your WSL distro is ready for your daily practical exercises :D

1. Run PySpark Interactive Shell
~~~bash
      $ cd ~
      $ pyspark
~~~
2. Run jupyter notebook server after copying those required files from Windows (c:\de\sparkexercise) to Ubuntu (/home/hduser/sparkexercise) via the named directory sparkexercise. Then, copy and paste one of the URLs that are listed in any web browser
~~~bash
      $ sudo cp -r /mnt/c/de/sparkexercise /home/hduser
      $ sudo chown -r hduser:hduser -R /home/hduser/sparkexercise
      $ cd ~/sparkexercise
      $ jupyter notebook --port=8888 --no-browser
~~~
3. Run Kafka. An additional two services can be observed using command jps in this step, i.e. QuorumPeerMain (for Zookeeper) and Kafka processes
~~~bash
      $ cd ~/kafka
      $ bin/zookeeper-server-start.sh config/zookeeper.properties &
      $ bin/kafka-server-start.sh config/server.properties &
~~~
4. Run HBase. An additional two services can be observed using command jps in this step, i.e. HMaster and HRegionServer processes
~~~bash
      $ cd ~
      $ /home/hduser/hbase/bin/start-hbase.sh
~~~
5. Run HBase shell.
~~~bash
      $ /home/hduser/hbase/bin/hbase shell
~~~
6. Run HappyBase with Python. An additional one service can be observed using command jps in this step, i.e. ThriftServer process
~~~bash
      $ cd ~
      $ hbase thrift start -p 9090 &
~~~
7. Run Derby for Hive use. An additional one service can be observed using command jps in this step, i.e. NetworkServerControl process
~~~bash
      $ cd ~/derby/data
      $ nohup ~/derby/bin/startNetworkServer -h 0.0.0.0 &
~~~
8. Run Hive. An additional one service can be observed using command jps in this step, i.e. RunJar process
~~~bash
      $ cd ~
      $ /home/hduser/hive/bin/hiveserver2
~~~
9. Run Beeline shell. 
~~~bash
      $ cd ~
      $ /home/hduser/hive/bin/beeline
~~~

![exclamation_mark](https://github.com/choojun/choojun.github.io/assets/6356054/dd0eeedb-feac-476d-a69c-6a0aa31d4159) Remember to stop the DFS, YARN, and other started services (in reverse order) to avoid data corruption in HDFS before shutting down your PC. Read the required details from sections E to M above

## Summary of Software Requirements (and Tested Version)

| Section  | Hadoop |  Spark |       Scala      | Jupyter Notebook |  Kafka |  HBase | HappyBase |   Derby   |  Hive | Web browser | SSH and PDSH |     Internet    |          WSL         |
|:--------:|:------:|:------:|:----------------:|:----------------:|:------:|:------:|:---------:|:---------:|:-----:|:-----------:|:------------:|:---------------:|:--------------------:|
|     E    |  3.3.6 |        |                  |                  |        |        |           |           |       |    Needed   |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     F    | Needed |  3.5.1 |                  |                  |        |        |           |           |       |             |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     G    | Needed | Needed | Targeted on 2.13 |       6.4.8      |        |        |           |           |       |    Needed   |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     H    | Needed | Needed |                  |                  |        |        |           |           |       |             |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     I    | Needed | Needed |                  |      Needed      |        |        |           |           |       |    Needed   |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     J    | Needed |        |       2.13       |                  |  3.7.0 |        |           |           |       |             |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     K    | Needed | Needed | Targeted on 2.13 |                  |        |  2.5.7 |           |           |       |             |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     L    | Needed |        |                  |                  | Needed | Needed |   1.2.0   |           |       |             |    Needed    | Constant access | v2 with Ubuntu 22.04 |
|     M    | Needed | Needed |                  |                  |        |        |           | 10.14.2.0 | 3.1.3 |             |    Needed    | Constant access | v2 with Ubuntu 22.04 |

-----------------------------------------------------------

## References

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



