### Useful links 
* Rockscluster 6.2 documentation at URL http://central6.rocksclusters.org/roll-documentation/base/6.2/
* Condor and Java application at URL http://research.cs.wisc.edu/htcondor/manual/v7.6/2_8Java_Applications.html
* for GT4-WS setup on Rocks 4.2.1 at URL http://goc.pragma-grid.net/wiki/index.php/GT4_WSGRAM_on_Rocks
* for PRAGMA VOMRS setup Rocks at URL http://goc.pragma-grid.net/wiki/index.php/PRAGMA_VOMRS_resource_site_setup_at_SDSC
* for administrative guide: http://goc.pragma-grid.net/wiki/index.php/Resource_Site_Administrator_Guide
* Mobile Desktop Grid (MDG) at URL https://github.com/choojun/mdg/wiki: It offers a new research studying platform for computer science postgraduate student, such as adopting others intra-communication protocol, embedding meta-scheduler for more wider clustered resources recruitment and computational execution, and intelligent responsive to computational interruption process. There are couples of research improvement and hybrid-with-in-progress study can be done in MDG model design perspective, such as adopting others intra-communication protocol, embedding meta-scheduler for more wider clustered resources recruitment and computational execution, and intelligent responsive to computational interruption process.

Unable to run SGE job: denied: host is no submit host 
```
 #resolved by
 qconf -as <frontend-name>
 #example
 qconf -as mdgcluster3.cs.usm.my
```

Enabling RSH on Compute Nodes. The default Rocks configuration does not enable rsh commands or login to compute nodes. Instead, Rocks uses ssh as a drop in replacement for rsh. There may be some circustances where ssh does not have exactly the same semantics of rsh. Further, there may be some users that cannot modify their application to switch from rsh to ssh. If you are one of these users you may wish to enable rsh on your cluster. **Warning**: Enabling rsh on your cluster has serious security implicatations. While it is true rsh is limited to the private-side network this does not mean it is as secure as ssh. 
```
 #Enabling rsh is done by setting an attribute. To enable rsh on all compute nodes, execute:
 rocks set appliance attr compute rsh true
 #To apply this configuration change to the compute nodes, reinstall all your compute nodes (refer below commands). If you only want to enable rsh on a specific node (e.g., compute-0-0), execute:
 rocks set host attr compute-0-0 rsh true
 #To apply this configuration change to compute-0-0, reinstall compute-0-0.
```


Add a existing user to existing group 
```
 usermod -a -G ftpgroup tonyuser
```

Change existing user tony primary group to wwwgroup 
```
 usermod -g wwwgroup tony
```

Rebuild the distribution in 5.3 
```
 cd /export/rocks/install
 rocks create distro
```

Reinstall the nodes 5.3 
```
 rocks set host boot compute action=install
 rocks run host compute reboot
```
    
To enable yum install and update in Rocks 4.2.1 
```
 # at Rocks4.2.1 /etc/yum.conf
 [main]
 cachedir=/var/cache/yum
 debuglevel=2
 logfile=/var/log/yum.log
 pkgpolicy=newest
 distroverpkg=redhat-release
 tolerant=1
 exactarch=1
 
 [base]
 name=Red Hat Linux $releasever - $basearch - Base
 #baseurl=http://mirror.dulug.duke.edu/pub/yum-repository/redhat/$releasever/$basearch/
 baseurl=http://holmes.umflint.edu/centos/$releasever/os/$basearch/
 [updates]
 name=Red Hat Linux $releasever - Updates
 #baseurl=http://mirror.dulug.duke.edu/pub/yum-repository/redhat/updates/$releasever/
 baseurl=http://holmes.umflint.edu/centos/$releasever/updates/$basearch/
 [addons]
 name=Red Hat Linux $releasever - Addons
 baseurl=http://holmes.umflint.edu/centos/$releasever/addons/$basearch/
 [extras]
 name=Red Hat Linux $releasever - Extras
 baseurl=http://holmes.umflint.edu/centos/$releasever/extras/$basearch/
```

    
MonALISA configuration 
```
 # say /state/partition1/home/choojun/MonaLisa
 cp /export/home/choojun/MonaLisa/Service/CMD/MLD /etc/rc.d/init.d
 chkconfig --add MLD
 chkconfig --level 345 MLD on
 # test the result at http://localhost:6004/axis/services/MLWebService?wsdl
```
    
After setup, retrieve Pragma certs at rocks-200.sdsc.edu 
```
 [choojun@rocks-200 ~]$ /opt/pragma-ca/bin/grid-certreq -sv ra.pragma-grid.net:pragma-exp_ra -em cindy@sdsc.edu -recv 140
 trying to connect RA server : ra.pragma-grid.net (11412)
 request for exporting a certificate ... ok
 save a CA certificate file : /export/home/choojun/.globus/cacert.pem
 save a certificate file : /export/home/choojun/.globus/usercert.pem
 [choojun@rocks-200 ~]$ pwd
 /export/home/choojun
 [choojun@rocks-200 ~]$ /opt/pragma-ca/bin/grid-hostreq -sv ra.pragma-grid.net:pragma-exp_ra -recv 141 -dir /export/home/choojun/hostcerts.new
 trying to connect RA server : ra.pragma-grid.net (11412)
 request for exporting a certificate ... ok
 save a CA certificate file : /export/home/choojun/hostcerts.new/cacert.pem
 save a certificate file : /export/home/choojun/hostcerts.new/hostcert.pem
 [choojun@rocks-200 ~]$ 
```
    
at eagle.usm.my, install Pragma user cert 
```
 -bash-3.2$ pwd
 /state/partition1/home/choojun/.globus
 -bash-3.2$ scp rocks-200.sdsc.edu:~/.globus/userkey.pem .
 userkey.pem                                   100%  951     0.9KB/s   00:00
 -bash-3.2$ scp rocks-200.sdsc.edu:~/.globus/cacert.pem .
 cacert.pem                                    100% 1208     1.2KB/s   00:00   
 -bash-3.2$ scp rocks-200.sdsc.edu:~/.globus/usercert.pem .
 usercert.pem                                  100% 1216     1.2KB/s   00:00   
 -bash-3.2$ ls -al
 total 24
 drwx---r-x  4 choojun choojun 4096 Aug 31 12:06 .
 drwxr-xr-x 16 choojun choojun 4096 Aug 30 19:47 ..
 -rw-r--r--  1 choojun choojun 1208 Aug 31 12:06 cacert.pem
 drwx---r-x  5 choojun choojun 4096 Aug 19  2009 .gass_cache
 drwx------  6 choojun choojun 4096 Oct 16  2008 job
 -rw-r--r--  1 choojun choojun 1216 Aug 31 12:06 usercert.pem
 -bash-3.2$ 
```
    
at eagle.usm.my: install Pragma host cert 
```
 [root@eagle certificates]# pwd
 /etc/grid-security/certificates
 [root@eagle certificates]# wget http://ca.pragma-grid.net/ca-certs/5456d9ca.0 .
 --2010-08-31 12:11:59--  http://ca.pragma-grid.net/ca-certs/5456d9ca.0
 Resolving ca.pragma-grid.net (ca.pragma-grid.net)... 198.202.74.232
 Connecting to ca.pragma-grid.net (ca.pragma-grid.net)|198.202.74.232|:80... connected.
 HTTP request sent, awaiting response... 200 OK
 Length: 1208 (1.2K) [text/plain]
 Saving to: `5456d9ca.0'
 100%[======================================>] 1,208       --.-K/s   in 0s     
 2010-08-31 12:11:59 (115 MB/s) - `5456d9ca.0' saved [1208/1208]
 --2010-08-31 12:11:59--  http://./
 Resolving . (.)... failed: Name or service not known.
 wget: unable to resolve host address `.'
 FINISHED --2010-08-31 12:11:59--
 Downloaded: 1 files, 1.2K in 0s (115 MB/s)
 [root@eagle certificates]#
 
 [root@eagle certificates]# wget http://ca.pragma-grid.net/ca-certs/5456d9ca.signing_policy .
 --2010-08-31 12:12:53--  http://ca.pragma-grid.net/ca-certs/5456d9ca.signing_policy
 Resolving ca.pragma-grid.net (ca.pragma-grid.net)... 198.202.74.232
 Connecting to ca.pragma-grid.net (ca.pragma-grid.net)|198.202.74.232|:80... connected.
 HTTP request sent, awaiting response... 200 OK
 Length: 1276 (1.2K) [text/plain]
 Saving to: `5456d9ca.signing_policy'
 100%[======================================>] 1,276       --.-K/s   in 0s     
 2010-08-31 12:12:54 (135 MB/s) - `5456d9ca.signing_policy' saved [1276/1276]
 --2010-08-31 12:12:54--  http://./
 Resolving . (.)... failed: Name or service not known.
 wget: unable to resolve host address `.'
 FINISHED --2010-08-31 12:12:54--
 Downloaded: 1 files, 1.2K in 0s (135 MB/s)
 [root@eagle certificates]# 
 
 [root@eagle certificates]#
 [root@eagle certificates]# chmod 644 5456d9ca.0
 [root@eagle certificates]# chmod 644 5456d9ca.signing_policy
 [root@eagle certificates]# ls -al 5456d9*
 -rw-r--r-- 1 root root 1208 Jan 17  2009 5456d9ca.0
 -rw-r--r-- 1 root root 1276 Jan 17  2009 5456d9ca.signing_policy
 [root@eagle certificates]# date
 Tue Aug 31 12:15:33 MYT 2010
 [root@eagle certificates]# 
 
 -bash-3.2$ pwd
 /state/partition1/home/choojun
 -bash-3.2$ scp rocks-200.sdsc.edu:~/hostcerts.new/* .
 cacert.pem                                    100% 1208     1.2KB/s   00:00   
 hostcert.pem                                  100% 1265     1.2KB/s   00:00   
 hostkey.pem                                   100%  887     0.9KB/s   00:00   
 -bash-3.2$ ls
 cacert.pem  Desktop  hostcert.pem  hostkey.pem
 -bash-3.2$ sudo mv *.pem /etc/grid-security/
 Password:
 -bash-3.2$ sudo chmod 644 /etc/grid-security/cacert.pem
 -bash-3.2$ sudo chmod 600 /etc/grid-security/hostkey.pem
 -bash-3.2$ sudo chmod 644 /etc/grid-security/hostcert.pem
 -bash-3.2$
 -bash-3.2$ ls -al /etc/grid-security/*.pem
 -rw-r--r-- 1 choojun choojun 1208 Aug 31 12:20 /etc/grid-security/cacert.pem
 -rw-r--r-- 1 choojun choojun 1265 Aug 31 12:20 /etc/grid-security/hostcert.pem
 -rw------- 1 choojun choojun  887 Aug 31 12:20 /etc/grid-security/hostkey.pem
 -bash-3.2$ sudo chown root:root /etc/grid-security/cacert.pem
 -bash-3.2$ sudo chown root:root /etc/grid-security/hostcert.pem
 -bash-3.2$ sudo chown root:root /etc/grid-security/hostkey.pem 
 -bash-3.2$ ls -al /etc/grid-security/*.pem
 -rw-r--r-- 1 root root 1208 Aug 31 12:20 /etc/grid-security/cacert.pem
 -rw-r--r-- 1 root root 1265 Aug 31 12:20 /etc/grid-security/hostcert.pem
 -rw------- 1 root root  887 Aug 31 12:20 /etc/grid-security/hostkey.pem
 -bash-3.2$ 
```
    
testing after certs installation 
```
 -bash-3.2$ grid-proxy-init -debug
 
 User Cert File: /state/partition1/home/choojun/.globus/usercert.pem
 User Key File: /state/partition1/home/choojun/.globus/userkey.pem
 
 Trusted CA Cert Dir: (null)
 
 Output File: /tmp/x509up_u500
 Your identity: /O=grid/O=pragma/OU=USM/CN=Tan Choo Jun
 Enter GRID pass phrase for this identity:
 Creating proxy ..++++++++++++
 .........++++++++++++
 Done
 Your proxy is valid until: Wed Sep  1 00:49:06 2010
 -bash-3.2$ globusrun -a -r eagle.usm.my
 
 GRAM Authentication test failure: connecting to the job manager failed.  Possible reasons: job terminated, invalid job contact, network problems, ...
 -bash-3.2$ globus-job-run eagle.usm.my/jobmanager-sge /bin/hostname
 GRAM Job submission failed because the connection to the server failed (check host and port) (error code 12)
 -bash-3.2$ 
```

Update Python in Rocks Cluster
```
 # uncompress the downloaded the python2.7
 tar xfvz python2.7.tar.gz
 
 cd python2.7
 run ./configure –prefix=/share/apps/python2.7 –with-threads –enable-shared
 make
 make install
 create python27.sh in /etc/profile.d
 if [ -d /share/apps/python2.7/bin ]; then
 export PATH=/share/apps/python2.7/bin:$PATH
 fi
 
 create python27.conf in /etc/ld.so.conf.d
 /share/apps/python2.7/lib
 
 copy these files to all compute nodes:
 say, we locate python27.sh and python27.conf in /share/apps
 rocks run host "cp /share/apps/python27.sh /etc/profile.d"
 rocks run host "cp /share/apps/python27.conf /etc/ld.so.conf.d/"
 
 run ldconfig  on all nodes:
 rocks run host ldconfig
```
