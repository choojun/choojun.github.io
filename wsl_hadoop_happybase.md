# L. HappyBase Installation and Configuration [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1. Read more on HappyBase at URL https://happybase.readthedocs.io/en/latest/

## L1. Install HappyBase [![home](https://github.com/choojun/choojun.github.io/assets/6356054/947da4b4-f259-4b82-8961-07ca48b2811a)](wsl)

1.	Login as hduser, and we may access Hbase using Python. We use the HappyBase’ Python library, which accesses Hbase using a Thrift gateway.
2.	Install HappyBase, and verify the installed packages with the following commands
~~~bash
$ cd ~
$ pip3 install happybase
$ python3 -c 'import happybase'
~~~








