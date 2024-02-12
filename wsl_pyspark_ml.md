# H. Spark and Machine Learning
 
> 1. Spark has two machine learning libraries with different APIs, but including implementations of similar algorithms. These machine learning algorithms and utilities have been designed to scale across a cluster by way of inclusion of parallel algorithms in which operations can be applied in parallel across nodes.
> - Spark MLlib: the older API which has entered a maintenance/bug-fix only mode.
> - spark.ml: The newer library which is inspired by scikit-learn.
> 2. As of Spark 2.0, the RDD-based APIs (https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds) in the spark.mllib package have entered maintenance mode. The primary Machine Learning API for Spark is now the DataFrame-based API (https://spark.apache.org/docs/latest/sql-programming-guide.html) in the spark.ml package. Therefore, Apache Spark recommends using spark.ml instead of spark.mllib. Read more at URL https://spark.apache.org/docs/latest/ml-guide.html#announcement-dataframe-based-api-is-primary-api
> 3. [mllib.zip](https://github.com/choojun/choojun.github.io/files/14240398/mllib.zip)


## H1. Installation and Configuration
1.	Download the correct version of Spark from the Apache Spark downloads page (http://spark.apache.org/downloads.html), and check the compatible with your installed Hadoop
~~~bash
$ wget https://downloads.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3-scala2.13.tgz
~~~
