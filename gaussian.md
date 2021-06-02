Gaussian 03, an electronic structure modeling application, and for more detail see URL http://www.gaussian.com/

### g03 Installation

Assuming in Rocks 5.3/5.4, using root account create a user guser, set its password and su to the created account 
```
 adduser guser
 passwd guser
 su - guser
```

Using root account create a new group "g03group" 
```
 groupadd g03group
```

Using root account add the user guser to existing group g03group
```
 usermod -a -G g03group guser
```

Using root account change the user guser ssh environment. Replace the last string from bash to csh in file /etc/passwd as example below. The ? shows the sequence number of user or group creation in operating system. 

From
```
 guser:x:5??:1??::/home/guser:/bin/bash
```
To
```
 guser:x:5??:1??::/home/guser:/bin/csh
```


Using guser account create a file .cshrc at its root directory (/home/guser) with following lines. 
```
 if (! -e /state/partition1/g03scrdir/$USER) mkdir /state/partition1/g03scrdir/$USER
 setenv GAUSS_SCRDIR /state/partition1/g03scrdir/$USER
 setenv g03root /share/apps
 source $g03root/g03/bsd/g03.login
```

Using root account uncompress the g03 to directory /share/apps/g03 
```
 tar -xvf g03.tar
 tar -xzvf g03-linux/TAR/LI3_900X.TAZ -C /share/apps/
```

Finally, run the g03 using user guser



### Run g03 with test404.com in single node environment
```
 [guser@anicca ~]$ cat test404.com
 %nproc=4
 %mem=1000Mb
 #p B1LYP/6-31G(d,p) opt=tight freq test
 Gaussian Test Job 403:
 h2o
 01
 h
 o1d
 h 2 d 1hoh
 d 0.9815
 hoh 102.3095
 [guser@anicca ~]$ g03 test404.com
 [guser@anicca ~]$
```


### Run g03 with test404.com in distributed and parallel environment
If u are familiar with OpenMPI, u can make the g03 run in distributed and parallel environment with all available resources in cluster server. For MPICH2, u may try it by modifying sample based on guideline at http://eagle.usm.my/wordpress/?p=19 . Conceptually, the OpenMPI case can be dome using steps below:

* Make sure your test404.com is executable (change it via chmod 755 test404.com)
* Write a script called my.job with following lines 
```
 #!/bin/csh
 #$ -S /bin/csh
 g03 test404.com
```

Write a file called machines with follwing lines 
```
 compute-0-0
 compute-0-1
 compute-0-2
 compute-0-3
 compute-0-4
 compute-0-5
```
    
Write a script called myapp.sh with following lines 
```
 /opt/openmpi/bin/mpirun -np 5 -machinefile machines /share/apps/g03/g03 /home/guser/test404.com
```

Do the SGE job submission via command below. This step is for your production execution (after above 4 steps are working properly) 
```
 qsub myapp.sh
```

Delete the SGE job via command below 
```
 qdel <job_id>
```

### Hints within the lines
At above configuration

* OpenMPI makes use 6 compute nodes, refer number of nodes stated inside file machines
* The execution of OpenMPI runs in 5 compute nodes, refer parameter np inside file myapp.sh
* test404.com takes 4 core in its processing, refer parameter nproc inside file test404.com
