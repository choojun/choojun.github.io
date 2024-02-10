# F. HDFS Basic File Operations
 
> 1. This practical introduces the basic Hadoop Distributed File System (HDFS) operations. 
> 2. Login as hduser, and the HDFS shell can be invoked using 
> ~~~
>  $ hdfs dfs <args>  
> ~~~
> 3. To see the available commands in the shell
> ~~~
>  $ hdfs dfs -help
> ~~~

## F1. Exercise 1
1.	Download a file with file ID 122PnuKaSaA_OyYOKnxQOdlMc5awdyf5v from Google Drive
~~~
$ wget --no-check-certificate 'https://docs.google.com/uc?export=download&id=122PnuKaSaA_OyYOKnxQOdlMc5awdyf5v' -O shakespeare.txt
~~~

2.	Copy the downloaded file shakespeare.txt to the distributed file system
~~~
$ hdfs dfs -put shakespeare.txt shakespeare.txt
~~~

3.	Make a directory named corpora in the HDFS file system
~~~
$ hdfs dfs -mkdir corpora
~~~

4.	Read the contents of the file using the cat command, and then pipe the output to less in order to view the contents of the remote file
~~~
$ hdfs dfs -cat shakespeare.txt | less
~~~
> Use the arrow keys to navigate the file. Type q to quit.

5.	Copy a file from the distributed file system to the local file system.
~~~
$ hdfs dfs -get shakespeare.txt ./shakespeare-dfs.txt
~~~



## F2. Exercise 2
1.	To view the contents of your current directory
~~~
$ hdfs dfs -ls
~~~

2.	To view the contents of a non-empty directory
~~~
$ hdfs dfs -ls /user
~~~

3.	To create a test directory, e.g., testHDFS. Note that it will appear within the HDFS.
~~~
$ hdfs dfs -mkdir testHDFS
~~~

4.	Now you must verify that the created directory by listing your HDFS. You should see the testHDFS directory listed, if it is created at the home directory of hduser.
~~~
$ hdfs dfs -ls /user/hduser
~~~

5.	Create a file, which you wish to copy from local file system to HDFS
~~~
$ echo "HDFS test file" >> testFile
~~~

6.	Note that that is going to create a new file named testFile, including the characters HDFS test file. To verify this, input:
~~~
$ ls
~~~

7.	To verify that the file was created
~~~
$ cat testFile
~~~

8.	To copy the file to HDFS from Linux local file system into HDFS
~~~
$ hdfs dfs -copyFromLocal testFile
~~~
> Note that you have to use the command -copyFromLocal. The command -cp is used to copy files within HDFS.

9.	Now you need to confirm that the file has been copied over correctly
~~~
$ hdfs dfs -ls
$ hdfs dfs -cat testFile
~~~

10.	Now you can move it into the testHDFS directory you have already created
~~~
$ hdfs dfs -mv testFile testHDFS
$ hdfs dfs -ls
$ hdfs dfs -ls testHDFS/
~~~
> The first command moved your testFile from the HDFS home directory into the test one you created. The second command of this command then shows us that it's no longer in the HDFS home directory, and the third command confirms that it's now been moved to the test HDFS directory.

11.	To copy a file
~~~
$ hdfs dfs -cp testHDFS/testFile testHDFS/testFile2
$ hdfs dfs -ls testHDFS/
~~~

12.	Checking disk space is useful when you're using HDFS. It can be identified with the following command
~~~
$ hdfs dfs -du
~~~

13.	To view how much space is available in HDFS across the Hadoop cluster
~~~
$ hdfs dfs -df
~~~

14.	To to delete a file or directory in the HDFS
~~~
$ hdfs dfs -rm testHDFS/testFile
$ hdfs dfs -ls testHDFS/
~~~

15.	You will observe that you still have the testHDFS directory and testFile2 leftover that you created. Remove the directory with the following command
~~~
$ hdfs dfs -rm -r testhdfs
$ hdfs dfs -ls
~~~
> In addition to the above commands, there are a number of POSIX-like commands (https://en.wikipedia.org/wiki/List_of_POSIX_commands) which include chgrp, chmod, chown, cp, du, mkdir, stat, tail







