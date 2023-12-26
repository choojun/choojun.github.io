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





### B. Running Ubuntu on WSL

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




