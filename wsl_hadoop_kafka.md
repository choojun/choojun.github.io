![kafka](https://github.com/choojun/choojun.github.io/assets/6356054/9e24ef13-f41f-4d8a-a731-889cdb9f76df)

# J. Kafka Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Read more on Kafka at URL https://kafka.apache.org/36/documentation.html

## J1. Install Kafka [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

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
$ wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz
$ tar -xvzf kafka_2.13-3.7.0.tgz  
$ mv kafka_2.13-3.7.0 kafka
$ chown hduser:hadoop -R kafka
~~~

## J2. Starting and Stopping Zookeeper and Kafka Broker [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser. Ensure the following services started sequentially and check them with command jps
    - SSH
    - HDFS
    - YARN

2.	Start Zookeeper and Kafka broker server in the background
~~~bash
$ cd ~/kafka
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
> **Attention**: When you have finished your exercises (before shutdown your Windows), you need stop the Kafka and Zookeeper services




## J3. Producing and Examining Messages - using Terminal [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Login as hduser. 
2. Create a Topic
~~~bash
$ ~/kafka/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
~~~

3. List all available topics
~~~bash
$ ~/kafka/bin/kafka-topics.sh --list --bootstrap-server localhost:9092
~~~

4. Create another topic known as automobile.
5. List all available topics.
6. To obtain information about a specific Kafka topic
~~~
$ ~/kafka/bin/kafka-topics.sh --describe --bootstrap-server localhost:9092 --topic test
~~~



## J4. Producer - Consumer Terminals [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. To simulate a producer terminal, open another terminal of Ubuntu and login as hduser
2. List all available topics, and carry out the following steps after your Consumer Terminal is ready to consume messages

3. Producer terminal: Send a few messages under the topic test
~~~
$ ~/kafka/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
~~~
> A prompt (>) should appear for you to input your messages. For example, type in the following messages
> > This is the first test message
> > 
> > This is the second test message
> 
> To quit, press Ctrl-C. Note that messages are stored by default in the /tmp/kafka-logs/ directory or set as the value of log.dirs in the config/server.properties file.

4. Producer terminal: To delete a specific topic
~~~
$ ~/kafka/bin/kafka-topics.sh --delete --bootstrap-server localhost:9092 --topic test
~~~

5. To simulate a consumer terminal, open another terminal of Ubuntu and login as hduser
6. Consumer terminal: To consume messages by start a console-based consumer with command as follow
~~~
$ ~/kafka/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
~~~
> At your Producer terminal, send some messages (refer to Steo 3)

6. To examine the incoming messages
~~~
$ ~/kafka/bin/kafka-run-class.sh kafka.tools.DumpLogSegments --deep-iteration --print-data-log --files /tmp/kafka-logs/test-0/00000000000000000000.log
~~~
> Note that follow the file name of the *.log file that appears in your directory


## J5. Accessing Kafka in Python [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, and check the installed kafka-python package
~~~bash
$ cd ~
$ pip install kafka-python
~~~

2.	Use another terminal as consumer by login as hduser, and start the python interpreter
~~~bash
$ cd ~
$ python
~~~

3. In the consumer terminal, enter the following Python statements to consume messages
~~~python
>>> from kafka import KafkaConsumer
>>> consumer = KafkaConsumer(bootstrap_servers='localhost:9092')
>>> 
>>> for msg in consumer:
...     print("msg: ", msg)
~~~

4.	Use another terminal as producer by login as hduser, and start the python interpreter
~~~bash
$ cd ~
$ python
~~~

5. In the producer terminal, enter the following Python statements to generate messages
~~~python
>>> from kafka import KafkaProducer
>>> from json import dumps
>>> producer = KafkaProducer(bootstrap_servers=['localhost:9092'],value_serializer=lambda x: dumps(x).encode('utf-8'))
>>> producer.send('automobile', value='The new Ford Ranger')
>>> import time
>>> i = 1
>>> while True:
...     time.sleep(10)
...     print("i = ", i)
...     producer.send('test', str(i*10) + " seconds have passed")
...     i += 1
~~~

6. Further configure delay in sending out messages from producer terminal.
By default, a buffer is available to send the messages immediately even if there is still unused space in the buffer. To reduce the number of requests, you may set the linger_ms parameter of the KafkaProducer constructor to instruct the producer to wait up to the given number of ms before sending a request in order to enable more records to arrive for batch sending of the messages, thus leading to fewer and more efficient requests handling when not under the maximal load at the cost of a small amount of latency.
~~~python
>>> producer=KafkaProducer(linger_ms=8000, bootstrap_servers=['localhost:9092'], value_serializer=lambda x:dumps(x).encode('utf-8'))
>>> producer.flush()
>>> future=producer.send('test','test message 1')
>>> future=producer.send('test','test message 2')
>>> future=producer.send('test','test message 3')
~~~
> In the above example, the 8s delay will result in all three messages appearing at almost the same time to the consumer.


## J6. Accessing Kafka in Python - by Specifying Partition [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Use another terminal as producer by login as hduser, and invoke the Pyspark’s interactive shell with 4 executor cores
~~~bash
$ cd ~
$ pyspark --executor-cores 4
~~~

2. In the producer terminal, enter the following Python statements to generate messages
~~~python
>>> from kafka import KafkaProducer
>>> from json import dumps
>>> import time
>>> producer = KafkaProducer(bootstrap_servers=['localhost:9092'],value_serializer=lambda x: dumps(x).encode('utf-8'))
>>> i = 1
>>> while True:
...	  time.sleep(10)
...	  print("i = ", i)
...	  producer.send('test', str(i*10) + " seconds have passed", partition=0)
...     i += 1
~~~

3.	Use another terminal as consumer by login as hduser, and invoke Pyspark’s interactive shell
~~~python
>>> from kafka import KafkaConsumer                                                  
>>> from kafka import TopicPartition
>>> consumer = KafkaConsumer(bootstrap_servers='localhost:9092')
>>> consumer.assign([TopicPartition('test', 2)])
>>> consumer.assign([TopicPartition('test', 0)])
>>> for msg in consumer:
...     print("msg in consumer ", msg)
~~~


## J7. Accessing Kafka in Python - with Serialized JSON Messages [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. In producer terminal, update the following lines of code based on the copy of J6
~~~python
>>> i = 1                                               
>>> while True:
...     output = "Elapsed time: " + str(i * 10) + "s"
...     producer.send('test', {'message':output})
...     print('message ' + output + " sent")
...     time.sleep(10)
...     i += 1
~~~

2. Create a file named send_messages.py
~~~bash
$ cd ~
$ nano send_messages.py
~~~

3. Add the following statements into send_messages.py and then save the file
~~~python
from kafka import KafkaProducer
from json import dumps
import time
producer=KafkaProducer(bootstrap_servers=['localhost:9092'],value_serializer=lambda x: dumps(x).encode('utf-8'))
producer.send('automobile', value='The new Proton X70')
producer.send('automobile', value='Proton X50')
producer.flush()
~~~

4. Run the python file
~~~bash
$ python send_messages.py
~~~





## J8. Accessing Kafka in Python - Streaming Twitter Data [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, and check the installed kafka-python package
~~~bash
$ cd ~
$ pip install python-twitter
$ pip install tweepy
~~~

2. Get your API access keys from your Twitter API account. If you do not have a Twitter Developer account yet, you may apply for one at URL https://developer.twitter.com/en/apply-for-access

3. Create a python file with the following lines of code. Note that you have to replace access_token, access_token_secret, consumer_key, and consumer_secret in the code below with your own Twitter API account tokens and keys. 
~~~python
from tweepy.streaming import StreamListener
from tweepy import OAuthHandler
from tweepy import Stream
from kafka import KafkaProducer
import json
access_token = "__"
access_token_secret =  "__"
consumer_key =  "__"
consumer_secret =  "__"
class StdOutListener(StreamListener):
    def on_data(self, data):
        json_ = json.loads(data)
        producer.send("automobile", json_["text"].encode('utf-8'))
        return True

    def on_error(self, status):
        print(status)
producer = KafkaProducer(bootstrap_servers='localhost:9092')
l = StdOutListener()
auth = OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
stream = Stream(auth, l)
stream.filter(track=["automobile", "car", "transport", "train", "LRT"])
~~~

4. Run your file and observe the messages that arrive at the Consumer terminal

