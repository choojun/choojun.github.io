# J. Kafka Installation and Configuration
1. Read more on Kafka at URL https://kafka.apache.org/36/documentation.html

## J1. Install Kafka
1.	Login as hduser, and check the installed scala package
~~~bash
$ cd ~
$ scala -version
~~~

2.	You may install the required scala version (by Kafka) as steps as follows
~~~bash
$ cd ~
$ sudo apt install zip unzip
$ curl -s "https://get.sdkman.io" | bash
$ source "/home/hduser/.sdkman/bin/sdkman-init.sh"
$ sdk install scala 2.13.12
$ scala -version
~~~

3.	Download and setup the respective version of Kafka based on the installed version of Scala. The available Kafka versions may be found at the Kafka download page - https://kafka.apache.org/downloads
~~~bash
$ cd ~
$ wget https://downloads.apache.org/kafka/3.6.1/kafka_2.13-3.6.1.tgz
$ tar -xvzf kafka_2.13-3.6.1.tgz  
$ mv kafka_2.13-3.6.1 kafka
$ chown hduser:hadoop -R kafka
~~~

## J2. Starting and Stopping Kafka Broker
1.	Login as hduser. Ensure the following services started sequentially and check them with command jps
    - SSH (if using WSL). 
    - HDFS
    - YARN

2.	Start Zookeeper and Kafka broker server in the background
~~~bash
$ cd kafka
$ bin/zookeeper-server-start.sh config/zookeeper.properties &
(Then, press the <enter> key and Zookeeper will continue running in the background.)
$ bin/kafka-server-start.sh config/server.properties &
(Then, press the <enter> key and Kafka broker will continue running in the background.)
$ jps
~~~
> Note that you should observe the QuorumPeerMain (for Zookeeper) and Kafka processes with the command jps. You should observe a total of eight services, i.e. QuorumPeerMain, Jps, SecondaryNameNode, ResourceManager, NodeManager, DataNode, Kafka, NameNode

3.	Stopping Kafka and Zookeeper
~~~bash
$ bin/kafka-server-stop.sh
$ bin/zookeeper-server-stop.sh
~~~
> When you have finished running your exercises, you need stop the Kafka service


