![spark](https://github.com/choojun/choojun.github.io/assets/6356054/c357a221-ebe5-426c-9d9e-1be022fcfda5)

# I. Spark and Visualization [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Seaborn is a Python data visualization library based on matplotlib. It provides a high-level interface for drawing attractive and informative statistical graphics. Read more at URL https://seaborn.pydata.org/
2. [Visualization.zip](https://github.com/choojun/choojun.github.io/files/14373625/Visualization.zip)


## I1. Visualization with PySpark [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

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

3. Open up the Practical8_Visualization_with_Pyspark.ipynb notebook and do the visualization exercises there for the construction of pie chart, correlation matrix, boxplot, scatter chart, violin plot and 3D scatter plot



![pie](https://github.com/choojun/choojun.github.io/assets/6356054/18d5dfe5-3ea6-4a80-9c6e-fc068f36ab8a)
![boxplot](https://github.com/choojun/choojun.github.io/assets/6356054/70ff8053-5169-4017-8d3e-d297381cade5)
![scatter](https://github.com/choojun/choojun.github.io/assets/6356054/fba671b0-d50f-45e9-81d1-08dabb348cfa)
![violin](https://github.com/choojun/choojun.github.io/assets/6356054/a49bb96b-898c-430e-9eb6-8eef15d07b25)
![3dscatter](https://github.com/choojun/choojun.github.io/assets/6356054/b439d6aa-4164-4821-88c4-7a5f8fc49b6b)






