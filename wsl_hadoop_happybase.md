# L. HappyBase Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Read more on HappyBase at URL https://happybase.readthedocs.io/en/latest/

## L1. Install HappyBase [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, and we may access Hbase using Python. We use the HappyBase’ Python library, which accesses Hbase using a Thrift gateway.
2.	Install HappyBase, and verify the installed packages with the following commands
~~~bash
$ cd ~
$ pip3 install happybase
$ python3 -c 'import happybase'
~~~

3. Ensure the following services started sequentially, and check them with command jps
    - SSH
    - HDFS
    - YARN
    - Kafka
    - HBase
> Note that a total of ten (10) services can be observed eventually.

4. Start up the Thrift service at port 9090 to run at the background
~~~bash
$ cd ~
$ hbase thrift start -p 9090 &
~~~
> Find the pid for the Thrift service, and stop the Thrift service using the identified pid
~~~bash
$ ps -aux | grep 9090
hduser    9441  1.5  3.1 2928716 152960 pts/2  Sl   07:46   0:05
$ kill 9441
~~~
Or,
~~~bash
$ jps
6849 NodeManager
9441 ThriftServer
7233 QuorumPeerMain
8321 HMaster
6227 DataNode
6451 SecondaryNameNode
6053 NameNode
9735 Jps
8539 HRegionServer
6700 ResourceManager
7613 Kafka
$ kill 9441
~~~

## L2. Using HappyBase [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)
1.	Open another session in terminal, and login as hduser. Then, launch the python command-line interpreter
~~~bash
$ python
~~~
> When you want to exit the python command-line interpreter, type
~~~bash
>>> exit()
~~~

2. Creating connection to the HBase by importing HappyBase. Then, create connection to localhost
~~~bash
>>> import happybase
>>> connection = happybase.Connection(port=9090)
~~~

3. List all the available HBase tables
~~~bash
>>> print(connection.tables())
~~~

4. Display all the rows in one of the HBase tables
~~~bash
>>> t = connection.table('linkshare')
>>> rs = t.scan()
>>> for r in rs:
...     print(r)
~~~







