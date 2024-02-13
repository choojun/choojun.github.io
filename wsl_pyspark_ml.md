# H. Spark and Machine Learning
 
> 1. Spark has two machine learning libraries with different APIs, but including implementations of similar algorithms. These machine learning algorithms and utilities have been designed to scale across a cluster by way of inclusion of parallel algorithms in which operations can be applied in parallel across nodes.
>> - **spark.mllib**: spark.mllib is the older built-in Spark machine learning library. It consists of learning algorithms using Spark’s RDD operations. To leverage this, you need to create data (as RDDs) and operate on data in a distributed, parallelizable manner to make them available to all nodes in the cluster.
>> - **spark.ml**: The newer library which is inspired by scikit-learn. It provides a uniform set of high-level API built on top of DataFrames for constructing and tuning machine learning pipelines. 
> 2. As of Spark 2.0, the RDD-based APIs (https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds) in the spark.mllib package have entered maintenance mode. The primary Machine Learning API for Spark is now the DataFrame-based API (https://spark.apache.org/docs/latest/sql-programming-guide.html) in the spark.ml package. Therefore, Apache Spark recommends using spark.ml instead of spark.mllib. Read more at URL https://spark.apache.org/docs/latest/ml-guide.html#announcement-dataframe-based-api-is-primary-api
> 3. [mllib.zip](https://github.com/choojun/choojun.github.io/files/14240398/mllib.zip)


## H1. spark.mllib - Recommender Systems
1.	This example uses Alternating Least Squares (ALS) algorithm to obtain potential matches for an online dating service
2. Make a copy of the C:\de\mllib folder in the local hduser’s home directory
~~~bash
$ sudo cp -r /mnt/c/de/mllib /home/hduser
$ sudo chown hduser:hduser -R /home/hduser/mllib
~~~

3. Copy all the data files in the mllib/data directory to the data/ directory in the distributed file system.

4. Submit the matchmaker.py job to Spark and pipe the output to a file
~~~
$ $SPARK_HOME/bin/spark-submit mllib/matchmaker.py 1 M > matchmaking_recs.txt
~~~

## H2. spark.mllib - Clustering
1.	This example uses k-means clustering to determine which areas in the US have been most hit by earthquakes
2. Make a copy of the C:\de\mllib folder in the local hduser’s home directory
~~~bash
$ sudo cp -r /mnt/c/de/mllib /home/hduser
$ sudo chown hduser:hduser -R /home/hduser/mllib
~~~

## H3. spark.ml - Multiclass Classification
1.	This example uses the Naive Bayes classifier to perform multiclass classification
2. Make a copy of the C:\de\mllib folder in the local hduser’s home directory
~~~bash
$ sudo cp -r /mnt/c/de/mllib /home/hduser
$ sudo chown hduser:hduser -R /home/hduser/mllib
~~~

## H4. spark.ml - Recommender (Regression)
1.	This example uses Alternating Least Squares (ALS) algorithm for movie recommendations
2. Make a copy of the C:\de\mllib folder in the local hduser’s home directory
~~~bash
$ sudo cp -r /mnt/c/de/mllib /home/hduser
$ sudo chown hduser:hduser -R /home/hduser/mllib
~~~


