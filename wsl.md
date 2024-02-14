# Microsoft Windows Subsystem for Linux (WSL)

## A. Remove/Setup WSL (tested Windows 10 and WSL v2)

1. Hardware requirements: your PC need to have at least **16GB RAM (or more)** and **CPU virtualization enabled** with **internet access** for the rest of exercises.
Suppose that your Windows has updated to the latest patches (e.g., version 22H2 and later for Windows 10).  
Software reqirements:
   - Find Settings --> Apps --> Apps & features --> Related Settings --> Programs and Features --> Turn Windows features on or off, **or**
   - Find Settings --> System -->  Optional features --> Related Settings --> More Windows features --> Turn Windows features on or off.

3. Click (for setup) or unclick (for remove) the following items (if any available)
    - Hyper-V
    - Virtual Machine Platform
    - Windows Hypervisor Platform
    - Windows Subsystem for Linux.

4. Restart PC

5. Suppose that your PC accessible to the Internet (for huge files downloading), perform the search of required keyword, e.g., Ubuntu, in the Microsoft Store. Click on the Get button to download and intall the required distro, e.g., UBuntu-xx.xx, and laumch the installed distro via the created link in the Windows Start. A sudoer user will be created during the initial launching.   
![ubuntu](https://github.com/choojun/choojun.github.io/assets/6356054/d2a117c5-2471-4834-8cf2-d461ed3fbb5c)
> To set up the required Ubuntu distro (same version), run the following commands in PowerShell as administrator.
>
> ~~~
> wsl --update
> (restart PC)
>
> wsl –-list --online
>
> wsl --install -d UBuntu-xx.xx
> (restart PC)
> ~~~
>
> You may install a specific version of distro under the PowerShell as Administrator, e.g., the adopted version as PC either in lab or your course mate. 
>
> ![image](https://github.com/choojun/choojun.github.io/assets/6356054/e6259ec4-76fa-48a4-ab5f-0ba0b0b524c4)
>
> Locate your administrator username and password, after reboot and before first use.
>
> ![image](https://github.com/choojun/choojun.github.io/assets/6356054/7c24254b-a6c5-45a4-9973-6bace4969717)




## B. Running Ubuntu on WSL

1. Suppose that the required distro as default, run the following command with Power Shell.
~~~
wsl ~
~~~

2. Suppose that more than one distro setup in WSL, e.g. after importing backup distro, you may list them and run the particular distro (using created user account AND remember to change it home directory using command ‘cd  ~’) with commands as follows.
~~~
wsl –l -v
wsl –d <distro name> -u <created user in distro>
    e.g. wsl –d Ubuntu-22.04 –u tarumt
~~~

3. You may set the targeted distro as default with commands as follows.
~~~
wsl –-set-default <distro name>
    e.g. wsl –set-default Ubuntu-22.04
wsl –l -v
~~~

4. Even when issue ‘exit’ command, it does not turn off your WSL distro. You may terminate a running WSL distro with commands as follows.
~~~
wsl –t <distro name>
    e.g. wsl –t Ubuntu-22.04
~~~

5. Alternatively, you may issue the following command to terminate a running Linux, e.g. Ubuntu, as follows. Note that you need to provide a valid password (current user) when prompt. The WSL distro may terminate a few minutes later.
~~~
sudo shutdown –r 0
~~~





## C. Backup the Distro on WSL

1. In Power Shell, list the installed distro on WSL with the following command. Note that you need to know the exact name to create a backup
~~~
wsl –l -v
~~~

2. Change the directory that you want to save your backup with command ‘cd’. At the destination of directory, perform the following command to export the distribution of distro with Power Shell.
~~~
wsl --export <distribution> <filename.tar>
    e.g. wsl --export Ubuntu-22.04 my-backup-Ubuntu-22.04.tar
~~~

3. Alternatively, instead of using ‘cd’ to get to the destination directory, you may specify the file location and filename as part of export process in the Power Shell as follows.
~~~
wsl --export <distribution> <file location with filename.tar>
    e.g. wsl --export Ubuntu-22.04 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~




## D. Restore (Import) the Distro on WSL

1. In Power Shell, list the installed distro on WSL before removing the same / targeted instance. Note that you need to know the exact name for this process, especially those who wants to restore it at some point on the same PC or on those PC with the same name for distribution of distro. Note that the 'unregister' command will unregister the distribution from WSL and deletes the root filesystems.
~~~
wsl –l –v
wsl --unregister <targeted distribution>
~~~

2. Import the distribution of distro with Power Shell. Note that you may import the same distribution of distro into multiple install location, and this is good to facilitate your software development and testing purposes.
~~~
wsl --import <distribution> <*install location> <filename.tar>
    e.g. wsl --import Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~

3. In Power Shell, verify the distro has been imported correctly.
~~~
wsl –l -v
~~~

4. The beauty of using both export and import commands is that you can quickly and easily setup the same environment on multiple machines or setup multiple distros in the same machine. Your users and passwords will be retained, including anything you have installed using the package manager.


-----------------------------------------------------------

## E. [Hadoop Installation and Configuration](wsl_hadoop)
## F. [HDFS File Operations and MapReduce](wsl_hdfs)
## G. [Spark, PySpark, Spark SQL and Jupyter Notebook](wsl_pyspark)
## H. [Spark and Machine Learning](wsl_pyspark_ml)
## I. [Spark and Visualization](wsl_pyspark_viz)
-----------------------------------------------------------

## Reference:

1. https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

2. https://github.com/apache/spark/tree/master

