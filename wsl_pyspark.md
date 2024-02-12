# G. PySpark
 
> 1. Spark is an in-memory distributed computing engine especially designed for the Hadoop cluster. 
> 2. Read more on Apache Spark at URLs https://spark.apache.org and https://en.wikipedia.org/wiki/Apache_Spark
> 3. [SparkApp.zip](https://github.com/choojun/choojun.github.io/files/14237887/SparkApp.zip)


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
> To exit the pyspark shell with command exit()

6. To minimize the verbosity of Spark , you need to configure the settings of log4j after duplicating the template file (optional).
~~~
$ cp $SPARK_HOME/conf/log4j2.properties.template $SPARK_HOME/conf/log4j2.properties
~~~
> Edit the newly created log4j file, by replacing every occurrence of INFO with WARN, e.g.
> rootLogger.level = warn  
> rootLogger.appenderRef.stdout.ref = console  


## G2. Using the PySpark Interactive Shell - Word Count 
1.	Launch PySpark’s interactive shell using user hduser
2.	Create an Resilient Distributed Dataset (RDD) with data from a text file. It transforms the RDD to implement the word count application using Spark.
~~~
>>> text = sc.textFile("shakespeare.txt")
>>> print(text)
>>> from operator import add
>>> def tokenize(text): return text.split()
...
>>> words = text.flatMap(tokenize)
~~~

3.	It applies the map, and then it applies the reduceByKey action to obtain the word counts before saving the results in file
~~~
>>> wc = words.map(lambda x: (x, 1))
>>> print(wc.toDebugString())
>>> counts = wc.reduceByKey(add)
>>> counts.saveAsTextFile("hdfs://localhost:9000/user/hduser/wc")
>>> exit()
~~~
> Apache spark version 3.3.6 does not need to have the hdfs directory path (hdfs://localhost:9000/user/hduser/) explicitly indicated to write in the local path, i.e., /home/hduser. Example:
> counts.saveAsTextFile("wc")

4.	Check the HDFS current working directory for the newly created directory’s contents, and use the head command to view one of the part files
~~~
$ hdfs dfs -ls wc
$ hdfs dfs -head wc/part-00000
~~~




## G3. Using the PySpark Interactive Shell - Numerical 
1.	Install numpy using user hduser, and launch numpy with 4 cores
~~~
$ pip3 install numpy
$ pyspark --executor-cores 4
~~~

2.	Launch PySpark’s interactive shell and try the following codes. The codes generate a list of random integers and create an RDD by parallelizing num_list with library numpy.
~~~
>>> import numpy as np
>>> num_list = np.random.randint(0, 10, 20)
>>> numbers_RDD = sc.parallelize(num_list)
>>> numbers_RDD
~~~

3.	You may check the type for numbers_RDD, return all the elements of the RDD as an array, return an RDD created by consolidating all elements within each partition into a list
~~~
>>> type(numbers_RDD)
>>> numbers_RDD.collect()
>>> numbers_RDD.glom().collect()
~~~

4.	You may extract various elements of the RDD and extract the unique values of the RDD
~~~
>>> numbers_RDD.first()
>>> numbers_RDD.take(3)
>>> numbers_RDD.take(5)
>>> numbers_RDD.take(8)
>>> distinct_numbers_RDD = numbers_RDD.distinct()
>>> distinct_numbers_RDD.collect()
~~~

5.	You may sum all the elements in numbers_RDD and sum all the elements in numbers_RDD using reduce
~~~
>>> numbers_RDD.collect()
>>> numbers_RDD.sum()
>>> numbers_RDD.reduce(lambda x, y: x+y)
~~~

6.	You may find the max value in numbers_RDD and filter non-zero numbers that are divisible by 3
~~~
>>> numbers_RDD.reduce(lambda x, y: x if x > y else y)
>>> numbers_RDD.filter(lambda x:x%3==0 and x!=0).collect()
~~~

7.	Create a new RDD with the squares of all the elements in numbers_RDD
~~~
>>> numbers_RDD.collect()
>>> squares_RDD = numbers_RDD.map(lambda x:x*x)
>>> squares_RDD.collect()
~~~

8.	Generate a histogram for the elements in squares_RDD and do mapping with a regular Python function
~~~
>>> squares_RDD.histogram([x for x in range(0, 100, 10)])
>>> def square_if_odd(x):
...     if x%2==1:
...             return x*x
...     else:
...             return x
...
>>> numbers_RDD.map(square_if_odd).collect()
~~~

9. Extract the even and odd numbers from numbers_RDD. What is the type of the collection that is returned? Sort the resulting numbers for the odd and even numbers
~~~
>>> results = numbers_RDD.groupBy(lambda x:x%2).collect()
>>> results
>>> type(results)
>>> sorted([(x, sorted(y)) for (x, y) in results])
~~~

10. Prform the union(), intersection(), subtract(), cartesian()
~~~
>>> squares_RDD.collect()
>>> numbers_RDD.collect()
>>> union_RDD = sc.union([numbers_RDD, squares_RDD])
>>> union_RDD.collect()
~~~

11. cartesian() generates a Cartesian product of two collections and returns all the possible combinations of pairs between the elements in both collections
~~~
>>> numbers_RDD.cartesian(numbers_RDD).collect()
~~~



## G4. Using the PySpark Interactive Shell - Text Processing 
1.	Find the longest word in a sentence
~~~
>>> words = 'It is not that I am so smart but I stay with the questions much longer Albert Einstein'.split()
>>> print(words)
>>> words_RDD = sc.parallelize(words)
>>> words_RDD.reduce(lambda w, v: w if len(w) > len(v) else v)
~~~
or
~~~
>>> def findLongestWord(x, y):
...     if len(x) > len(y):
...             return x
...     else:
...             return y
...
>>> words_RDD.reduce(findLongestWord)
~~~

2.	Find the lexicographically largest word in a sentence
~~~
>>> def findLargestWord(x, y):
...     if x > y:
...             return x
...     else:
...             return y
...
>>> words_RDD.reduce(findLargestWord)
~~~

3.	Find the lexicographically smallest word in a sentence using lambda expression
~~~
>>> words_RDD.reduce(lambda w, v: w if w < v else v)
~~~

4.	Count the number of lines in a text file with certain letters. Note the cache() function pulls data sets into a cluster-wide in-memory cache, which is very useful when data is accessed repeatedly
~~~
>>> logFile = "file:///home/hduser/shakespeare.txt"
>>> logData = sc.textFile(logFile).cache()
>>> number_of_w = logData.filter(lambda s:'w' in s).count()
>>> number_of_c = logData.filter(lambda s:'c' in s).count()
>>> print("Lines with 'c':{}, lines with 'w':{}".format(number_of_c, number_of_w))
~~~




## G5. Using the PySpark Interactive Shell - Word Count (SparkApp)
1.	Make a copy of the C:\de\SparkApp folder in the local hduser’s home directory
~~~
$ sudo cp -r /mnt/c/de/SparkApp /home/hduser
$ sudo chown hduser:hduser /home/hduser/SparkApp
~~~

2.	Change directory to the SparkApp folder, and review the code in wordcount.py
~~~
$ cd /home/hduser/SparkApp
$ cat wordcount.py | more
~~~

3.	Submit the wordcount.py job to Spark (directly to the YARN resource manager)
~~~
$ spark-submit wordcount.py shakespeare.txt hdfs://localhost:9000/user/hduser/spark_wc
~~~

4.	You may check the results directory in the HDFS system and view the beginning portion of the output file part-00000
~~~
$ hdfs dfs -ls spark_wc
$ hdfs dfs -head spark_wc/part-00000 
~~~


## G6. Using the PySpark Interactive Shell - Flight Delay (SparkApp)
1.	Change directory to the SparkApp folder, and review the code in app.py
~~~
$ cd /home/hduser/SparkApp
$ cat app.py | more
~~~

2.	Copy the folder with the data files to the distributed file system, and make a copy of the source file 
~~~
$ hdfs dfs -put SparkApp/ontime
$ sudo cp app.py app2.py
~~~

3.	Modify the file path in app.py, which has the codes of loading the airlines lookup dictionary and reading the CSV data into an RDD,  as follows
~~~
airlines = dict(sc.textFile("file:///home/hduser/SparkApp/ontime/airlines.csv").map(split).collect())
flights = sc.textFile("file:///home/hduser/SparkApp/ontime/flights.csv").map(split).map(parse)
~~~
to
~~~
airlines = dict(sc.textFile("ontime/airlines.csv").map(split).collect())
flights = sc.textFile("ontime/flights.csv").map(split).map(parse)
~~~

4.	Submit the wordcount.py job to Spark (directly to the YARN resource manager)
~~~
$ spark-submit app.py
~~~

5.	You may check the results directory in the HDFS 



## G7. Spark SQL
1.	Spark SQL is a module in Apache Spark that provides a relational interface to work with structured data using SQL-based operations in Spark through either JDBC/ODBC connectors, a built-in interactive Hive console, or Spark SQL API. This practical covers the Spark SQL API, which runs as a library on top of Spark’s Core engine and APIs. The Spark SQL API may be accessed using the same programming interface that was used for Spark’s RDD APIs  
![sparksql](https://github.com/choojun/choojun.github.io/assets/6356054/538f24de-c002-44a0-8b57-e17b107ce1c8)

2. Login as hduser, and install the Jupyter notebook, pandas, and reverse geocoder (https://pypi.org/project/reverse_geocoder/) packages 
~~~
$ sudo apt install jupyter-notebook
$ pip3 install pandas
$ pip3 install reverse_geocoder
~~~


