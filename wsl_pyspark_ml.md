![spark](https://github.com/choojun/choojun.github.io/assets/6356054/ed7bd2cd-1ce6-43db-8ebd-18d56f351a43)

# H. Spark and Machine Learning [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Spark has two machine learning libraries with different APIs, but including implementations of similar algorithms. These machine learning algorithms and utilities have been designed to scale across a cluster by way of inclusion of parallel algorithms in which operations can be applied in parallel across nodes.
 * **spark.mllib**: spark.mllib is the older built-in Spark machine learning library. It consists of learning algorithms using Spark’s RDD operations. To leverage this, you need to create data (as RDDs) and operate on data in a distributed, parallelizable manner to make them available to all nodes in the cluster.
 * **spark.ml**: The newer library which is inspired by scikit-learn. It provides a uniform set of high-level API built on top of DataFrames for constructing and tuning machine learning pipelines. 
2. As of Spark 2.0, the RDD-based APIs (https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds) in the spark.mllib package have entered maintenance mode. The primary Machine Learning API for Spark is now the DataFrame-based API (https://spark.apache.org/docs/latest/sql-programming-guide.html) in the spark.ml package. Therefore, Apache Spark recommends using spark.ml instead of spark.mllib. Read more at URL https://spark.apache.org/docs/latest/ml-guide.html#announcement-dataframe-based-api-is-primary-api
3. [mllib.zip](https://github.com/choojun/choojun.github.io/files/14240398/mllib.zip)


## H1. spark.mllib - Recommender Systems [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	This example uses Alternating Least Squares (ALS) algorithm to obtain potential matches for an online dating service
2. Make a copy of the C:\de\mllib folder in the local hduser’s home directory
~~~bash
$ sudo cp -r /mnt/c/de/mllib /home/hduser
$ sudo chown hduser:hduser -R /home/hduser/mllib
~~~

3. Copy all data files in the mllib directory, including its sub-directory mllib/data, to the data directory in the distributed file system. You may review the contents of the mllib and mllib/data directories before proceeding to the next step
~~~bash
$ hdfs dfs -put mllib /user/hduser
$ hdfs dfs -ls /user/hduser/mllib
$ hdfs dfs -ls /user/hduser/mllib/data
~~~

4. Review and understand the code in matchmaker.py before submitting the matchmaker.py job to Spark and piping the output to a file
~~~bash
$ hdfs dfs -cat /user/hduser/mllib/matchmaker.py | more
$ $SPARK_HOME/bin/spark-submit mllib/matchmaker.py 1 M > matchmaking_recs.txt
~~~
> Note that you need change the correct file paths inside the matchmaker.py, e.g.
> from **data/\*.dat** to **hdfs://localhost:9000/user/hduser/mllib/data/\*.dat**

5. Examine the output in the file matchmaking_recs.txt

## H2. spark.mllib - Clustering [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	This example uses k-means clustering to determine which areas in the US have been most hit by earthquakes
2. Ensure that the data file earthquakes.csv exists in HDFS’ data directory. You may review the contents of the mllib/data directory before proceeding to the next step
~~~bash
$ hdfs dfs -ls /user/hduser/mllib/data
~~~

3. Review and understand the code in earthquakes_clustering.py before submitting the earthquakes_clustering.py job to Spark and piping the output to a file
~~~bash
$ hdfs dfs -cat /user/hduser/mllib/earthquakes_clustering.py | more
$ $SPARK_HOME/bin/spark-submit mllib/earthquakes_clustering.py hdfs://localhost:9000/user/hduser/mllib/data/earthquakes.csv 6 > clusters.txt
~~~
> Note that this exercise takes the data file name, located in the distributed file system, as a parameter

4. Examine the output in the file cluster.txt

## H3. spark.ml - Multiclass Classification [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	This example uses the Naive Bayes classifier to perform multiclass classification
2. Ensure that the data file sample_libsvm_data.txt exists in HDFS’ data directory. You may review the contents of the mllib/data directory before proceeding to the next step
~~~bash
$ hdfs dfs -ls /user/hduser/mllib/data
~~~

3. Review and understand the code in naive_bayes_example.py before submitting the naive_bayes_example.py job to Spark
~~~bash
$ hdfs dfs -cat /user/hduser/mllib/naive_bayes_example.py | more
$ $SPARK_HOME/bin/spark-submit mllib/naive_bayes_example.py
~~~
> Note that you need change the correct file path inside the naive_bayes_example.py, e.g.
> from **data/\*.txt** to **hdfs://localhost:9000/user/hduser/mllib/data/\*.txt**

4. Examine the output in the terminal

## H4. spark.ml - Recommender (Regression based) [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	This example uses Alternating Least Squares (ALS) algorithm for movie recommendations
2. Ensure that the data file sample_movielens_ratings.txt exists in HDFS’ data directory. You may review the contents of the mllib/data directory before proceeding to the next step
~~~bash
$ hdfs dfs -ls /user/hduser/mllib/data
~~~

3. Review and understand the code in als_example.py before submitting the naive_bayes_example.py job to Spark
~~~bash
$ hdfs dfs -cat /user/hduser/mllib/als_example.py | more
$ $SPARK_HOME/bin/spark-submit mllib/als_example.py
~~~
> Note that you need change the correct file path inside the als_example.py, e.g.
> from **data/\*.txt** to **hdfs://localhost:9000/user/hduser/mllib/data/\*.txt**

4. Examine the output in the terminal

