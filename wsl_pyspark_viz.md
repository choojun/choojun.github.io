# I. Spark and Visualization

## I1. Visualization with PySpark
1.	Login as hduser, and install the seaborn package
~~~bash
$ cd ~
$ pip install seaborn
~~~

2. Make a copy of the C:\de\Visualization folder in the local hduser’s home directory
~~~bash
$ sudo cp -r /mnt/c/de/Visualization /home/hduser
$ sudo chown hduser:hduser -R /home/hduser/Visualization
~~~

2. Change to the Visualization directory and start the Jupyter notebook server by issuing the following command. Then, copy and paste one of the URLs that are listed in any web browser
~~~bash
$ cd ~/Visualization
$ jupyter notebook --port=8888 --no-browser
~~~
> To terminate the Jupyter notebook server with Ctrl-C

3. Open up the Practical8_Visualization_with_ Pyspark.ipynb notebook and do the Spark SQL exercises there
