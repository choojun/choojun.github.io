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
1.	Login as hduser, and ensure the following services started
    - SSH (if using WSL). 
    - HDFS
    - YARN



