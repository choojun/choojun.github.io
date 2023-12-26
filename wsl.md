# Microsoft Windows Subsystem for Linux (WSL)

## A. Remove/Setup WSL in Windows 10

1. Find Settings --> Apps --> Optional features 
Suppose that your PC accessible to Internet (for huge files downloading).
Click on the More Windows features under the Related settings section.
Click (for setup) or unclick (for remove) all items Hyper-V, Windows Hypervisor Platform and Windows Subsystem for Linux.

3. Restart PC

4. Suppose that your PC accessible to Internet (for huge files downloading), run the PowerShell as Administrator, and run the following command for setup the required Ubuntu distro

~~~
wsl --update
(restart PC)

wsl –list --online

wsl --install -d UBuntu-xx.xx
(restart PC)
~~~

You may install a specific version of distro under the PowerShell as Administrator, e.g., the adopted version as PC either in lab or your course mate. 

![image](https://github.com/choojun/choojun.github.io/assets/6356054/e6259ec4-76fa-48a4-ab5f-0ba0b0b524c4)

Locate your administrator username and password after reboot and before first use.

![image](https://github.com/choojun/choojun.github.io/assets/6356054/7c24254b-a6c5-45a4-9973-6bace4969717)





## B. Running Ubuntu on WSL

1. Suppose that the required distro as default, run the following command with Power Shell.
~~~
wsl ~
~~~

2. Suppose that more than one distro setup in WSL, e.g. after importing backup distro, you may list them and run the particular distro (using created user account AND remember to change it home directory using command ‘cd  ~’) with commands as follows.
~~~
wsl –l -v
wsl –d <distro name> -u <created user in distro>
    e.g. wsl –d Ubuntu-22.04 –u choojun
~~~

3. You may set the targeted distro as default with commands as follows.
~~~
wsl –-set-default (distro name)
    e.g. wsl –set-default Ubuntu-22.04
wsl –l -v
~~~

4. Even when issue ‘exit’ command, it does not turn off your WSL distro. You may terminate a running WSL distro with commands as follows.
~~~
wsl –t (distro name)
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
wsl --export (distribution) (filename.tar)
    e.g. wsl --export Ubuntu-22.04 my-backup-Ubuntu-22.04.tar
~~~

3. Alternatively, instead of using ‘cd’ to get to the destination directory, you may specify the file location and filename as part of export process in the Power Shell as follows.
~~~
wsl --export (distribution) (file location with filename.tar)
    e.g. wsl --export Ubuntu-22.04 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~




## D. Restore (Import) the Distro on WSL

1. In Power Shell, list the installed distro on WSL before removing the same / targeted instance. Note that you need to know the exact name for this process, especially those who wants to restore it at some point on the same PC or on those PC with the same name for distribution of distro. Note that the 'unregister' command will unregister the distribution from WSL and deletes the root filesystems.
~~~
wsl –l –v
wsl --unregister (targeted distribution)
~~~

2. Import the distribution of distro with Power Shell. Note that you may import the same distribution of distro into multiple install location, and this is good to facilitate your software development and testing purposes.
~~~
wsl --import (distribution) (*install location) (filename.tar)
    e.g. wsl --import Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/Ubuntu-22.04-test1 c:/Users/choojun/Documents/wsl/my-backup-Ubuntu-22.04.tar
~~~

3. In Power Shell, verify the distro has been imported correctly.
~~~
wsl –l -v
~~~

4. The beauty of using both export and import commands is that you can quickly and easily setup the same environment on multiple machines or setup multiple distros in the same machine. Your users and passwords will be retained, including anything you have installed using the package manager.



-----------------------------------------------------------

## E. Hadoop Installation and Configuration

Note:
1. Throughout our practical, we will assume that the Hadoop user name is hduser.
2. The first time that you launch your WSL Linux distro, you will be prompted to create a default user account. You may choose to either 
    * name the default user account as hduser, or 
    * create a separate user account named hduser.

### E1. Setup User Environment
1.	Create a new group named hadoop
~~~
$ sudo addgroup hadoop
~~~

2.	Create a new user account named hduser (if applicable)
Add the new user:
~~~
$ sudo adduser hduser
~~~

3. Grant the user sudo privileges
~~~
$ sudo usermod -aG sudo hduser
~~~

4.	Add hduser to the hadoop group
~~~
$ sudo usermod -a -G hadoop hduser
~~~

5.	Switch to the user account hduser (if applicable)
~~~
$ su - hduser
~~~

6.	If you want to go back to your original user session
~~~
$ exit
~~~

### E2. Setup Operating System Environment
1.	Reboot/terminate Ubuntu in WSL, and run the following commands from Ubuntu with user hduser.

2. Check if ssh has been installed
~~~
$ service ssh status
~~~

3. If ssh has not been installed, install ssh
~~~
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install ssh
~~~

4. Install pdsh
~~~
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install pdsh
~~~

5. Install Python (if necessary)
~~~
$ python --version 
$ sudo apt install software-properties-common
$ sudo apt update
$ sudo apt install python3
$ sudo apt install python3-dev
$ sudo apt install python3-wheel


6. Check current python versions and symlinks
~~~
$ ls -l /usr/bin/python* 
~~~

7. Set environment variables by editing your ~/.bashrc file:
~~~
$ sudo nano ~/.bashrc
~~~

8. In ~/.bashrc file, set Python 3 as the default python version, add the following command to set Python 3 as the default python version:
~~~
alias python=python3
~~~

9. In ~/.bashrc file, add the following lines at the end of the file based on the python3.x from your installation:
~~~
export CPATH=/usr/include/python3.10m:$CPATH
export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH
~~~

10. Save the file.

11. Source the file:
~~~
$ source ~/.bashrc
~~~

12. Install pip
~~~
$ sudo apt update
$ sudo apt install python3-pip
$ pip3 --version
$ sudo pip3 install --upgrade pip
~~~~





