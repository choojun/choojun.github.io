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

## F1. Execise 1
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



