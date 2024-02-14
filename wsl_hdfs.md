# F. HDFS File Operations and MapReduce
 
> 1. This practical introduces the basic Hadoop Distributed File System (HDFS) operations. 
> 2. Login as hduser, and the HDFS shell can be invoked using 
> ~~~bash
>  $ hdfs dfs <args>  
> ~~~
> 3. To see the available commands in the shell
> ~~~bash
>  $ hdfs dfs -help
> ~~~

## F1. Basic File Operations
1.	Download a file with file ID 122PnuKaSaA_OyYOKnxQOdlMc5awdyf5v from Google Drive
~~~bash
$ wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=122PnuKaSaA_OyYOKnxQOdlMc5awdyf5v' -O shakespeare.txt
~~~

2.	Copy the downloaded file shakespeare.txt to the distributed file system
~~~bash
$ hdfs dfs -put shakespeare.txt shakespeare.txt
~~~
> You may apply the option -f to force overwrite the destination file in the distributed file system, e.g.
> ~~~bash
> $ hdfs dfs -put -f shakespeare.txt shakespeare.txt
> ~~~~

3.	Make a directory named corpora in the HDFS file system
~~~bash
$ hdfs dfs -mkdir corpora
~~~

4.	Read the contents of the file using the cat command, and then pipe the output to less in order to view the contents of the remote file
~~~bash
$ hdfs dfs -cat shakespeare.txt | less
~~~
> Use the arrow keys to navigate the file. Type q to quit.

5.	Copy a file from the distributed file system to the local file system
~~~bash
$ hdfs dfs -get shakespeare.txt ./shakespeare-dfs.txt
~~~

6.	To change the permission of the file shakespeare.txt to 664
~~~bash
$ hdfs dfs -chmod 664 shakespeare.txt 
~~~
> 664 is an octal representation of the flags to set for the permission triple. The above statement  changes the permissions to -rw-rw-r--.  
> 6 is 110, which means read and write, but not execute.  
> 7 is 111, which means complete permissions.  
> 4 is 100, which means read-only.  

## F2. Advanced File Operations
1.	To view the contents of your current directory
~~~bash
$ hdfs dfs -ls
~~~

2.	To view the contents of a non-empty directory
~~~bash
$ hdfs dfs -ls /user
~~~

3.	To create a test directory, e.g., testHDFS. Note that it will appear within the HDFS.
~~~bash
$ hdfs dfs -mkdir testHDFS
~~~

4.	Now you must verify that the created directory by listing your HDFS. You should see the testHDFS directory listed, if it is created at the home directory of hduser.
~~~bash
$ hdfs dfs -ls /user/hduser
~~~

5.	Create a file, which you wish to copy from local file system to HDFS
~~~bash
$ echo "HDFS test file" >> testFile
~~~

6.	Note that that is going to create a new file named testFile, including the characters HDFS test file. To verify this, input:
~~~bash
$ ls
~~~

7.	To verify that the file was created
~~~bash
$ cat testFile
~~~

8.	To copy the file to HDFS from Linux local file system into HDFS
~~~bash
$ hdfs dfs -copyFromLocal testFile
~~~
> To copy files from your local machine to HDFS, use the command -copyFromLocal. The command -cp is only used to copy files within HDFS. For more options and flexibility in copying files or directories to your desired destination within HDFS, consider using the alternative command shown below
> ~~~bash
> hdfs dfs -put <Linux local file system> <distributed file system>
> ~~~

9.	Now you need to confirm that the file has been copied over correctly
~~~bash
$ hdfs dfs -ls
$ hdfs dfs -cat testFile
~~~

10.	Now you can move it into the testHDFS directory you have already created
~~~bash
$ hdfs dfs -mv testFile testHDFS
$ hdfs dfs -ls
$ hdfs dfs -ls testHDFS/
~~~
> The first command moved your testFile from the HDFS home directory into the test one you created. The second command of this command then shows us that it's no longer in the HDFS home directory, and the third command confirms that it's now been moved to the test HDFS directory.

11.	To copy a file
~~~bash
$ hdfs dfs -cp testHDFS/testFile testHDFS/testFile2
$ hdfs dfs -ls testHDFS/
~~~

12.	Checking disk space is useful when you're using HDFS. It can be identified with the following command
~~~bash
$ hdfs dfs -du
~~~

13.	To view how much space is available in HDFS across the Hadoop cluster
~~~bash
$ hdfs dfs -df
~~~

14.	To to delete a file or directory in the HDFS
~~~bash
$ hdfs dfs -rm testHDFS/testFile
$ hdfs dfs -ls testHDFS/
~~~

15.	You will observe that you still have the testHDFS directory and testFile2 leftover that you created. Remove the directory with the following command
~~~bash
$ hdfs dfs -rm -r testhdfs
$ hdfs dfs -ls
~~~
> In addition to the above commands, there are a number of POSIX-like commands (https://en.wikipedia.org/wiki/List_of_POSIX_commands) which include chgrp, chown, cp, du, mkdir, stat, tail


## F3. MapReduce with Java
1.	Hadoop MapReduce is a software framework for easily writing applications which process vast amounts of data (multi-terabyte data-sets) in-parallel on large clusters (thousands of nodes) of commodity hardware in a reliable, fault-tolerant manner. A MapReduce job splits the input data-set into independent chunks which are processed by the map tasks in a completely parallel manner. The framework sorts the outputs of the maps, which are then input to the reduce tasks. Typically both the input and the output of the job are stored in a file-system. The framework takes care of scheduling tasks, monitoring them and re-executes the failed tasks. The MapReduce framework operates exclusively on <key, value> pairs, that is, the framework views the input to the job as a set of <key, value> pairs and produces a set of <key, value> pairs as the output of the job, conceivably of different types.
2.	Input and Output types of a MapReduce job:
> (input) <k1, v1> -> map -> <k2, v2> -> combine -> <k2, v2> -> reduce -> <k3, v3> (output)

3.	Read more on MapReduce at URLs https://hadoop.apache.org/docs/r3.3.6/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html and https://en.wikipedia.org/wiki/MapReduce

4.	Make a copy of the C:\de\WordCount folder in the local hduser’s home directory
~~~bash
$ sudo cp -r /mnt/c/de/WordCount /home/hduser
$ sudo chown hduser:hduser -R /home/hduser/WordCount
~~~

5. Change directory to the WordCount directory
$ cd WordCount

## F4. MapReduce with Python
1.	To view the contents of your current directory
~~~bash
$ hdfs dfs -ls
~~~



