# G. PySpark
 
> 1. Spark is an in-memory distributed computing engine especially designed for the Hadoop cluster. 
> 2. Read more on Apache Spark at URL https://en.wikipedia.org/wiki/Apache_Spark

## G1. Installation and Configuration
1.	Download the correct version of Spark from the Apache Spark downloads page (http://spark.apache.org/downloads.html), and check the compatible with your installed Hadoop
~~~
$ wget https://downloads.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3-scala2.13.tgz
~~~

2.	Uncompress the file
~~~
$ tar -xvzf spark-3.5.0-bin-hadoop3-scala2.13.tgz
~~~

3.	Rename the Spark folder as spark
~~~
$ mv spark-3.5.0-bin-hadoop3-scala2.13 spark
~~~

4.	Edit the file ~/.bashrc with the the following lines
~~~
export SPARK_HOME=/home/hduser/spark
export PATH=$SPARK_HOME/bin:$PATH
~~~

5.	Re-load the environment
~~~
source ~/.bashrc
~~~

5.	Launch the PySpark interactive shell/interpreter
~~~
pyspark
~~~
> To exit the pyspark shell:
> >>> exit()




