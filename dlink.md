Add-ons for DNS-320, DNS-320L, DNS-325, DNS-327L, DNS-345, DNS-340L at URL http://dlink.vtverdohleb.org.ua/Add-On/

### For both DNS-323 and DNS-343 Administration

Samba client in Fedora 
```
 # Make Fedora/Ubuntu authenticate with your DNS-323/DNS-343, using the weak lanmanager style authentication that the DNS-323/DNS-343 expects.
 # To make this work, you need to edit the samba configuration. Open it up for editing by doing:
 # Ubuntu: gksu gedit /etc/samba/smb.conf
 # Fedora: gedit /etc/samba/smb.conf
 # Find the [global] section, and insert this line:
 client lanman auth = yes
 # Backup remote samba files/directories via command
 smbclient \\\\172.16.1.3\\remote_samba_share_dir -U choojun -Tc backup.pdf995.tar pdf995/
```

Enable NFS in DNS-323 or DNS-343 
```
 # NFS in DNS-323 (Source: http://forum.dsmg600.info/t1871-Req%3A-Full-instruction-install-DNS-323.html) ... with latest run_plug is not tested but can have a try! You need to start off by installing the fun-plug:
 1.  Go to http://www.inreto.de/dns323/fun-plug/0.4/ and download funplug-0.4.tar.gz
 2.  Open it up with winRAR (not winZIP) and place the 2 files onto your DNS-323 (drop them in the initial area where you have mapped your network drive to it).
 3.  Reboot your DNS-323 and wait until it's fully up and running again
 4.  Download putty.exe from http://www.chiark.greenend.org.uk/~sgta … nload.html
 5.  You can use putty to telnet into the DNS-323 by putting in the IP address of the DNS-323, port (23 is default for telnet), and choose the Telnet connection type.  Then click on the Open button.  A new window should appear that looks similar to a DOS command window.  You have now telnet'd into the root area of your DNS-323 as root.

 # Getting the NFS server up and running
 1.  Go to http://www.inreto.de/dns323/fun-plug/0.4/addons/ and download unfs3-0.9.18.tgz and portmap-6.0.tgz
 2.  Move the 2 .tgz files onto your DNS-323 (drop them in the initial area where you have mapped your network drive to it).
 3.  Telnet into your DNS-323 and go to the directory where you put your 2 .tgz files.  The command should be: 'cd /mnt/HD_a2' -- without the single quotes
 4.  Type 'funpkg.sh portmap-6.0.tgz'
 5.  Type 'funpkg.sh unfs3-0.9.18.tgz'
 6.  Type 'chmod 755 /mnt/HD_a2/fun_plug.d/start/unfsd.sh'
 7.  Type 'sh /mnt/HD_a2/fun_plug.d/start/unfsd.sh start' to start the NFS server -- next time, when you restart your DNS-323, the NFS server should automatically start

 # Use NFS in your Fedora (say at a VMware image)
 1. Make sure that the content of:
    etc/auto.home
    etc/auto.share
 is made empty (don't forget to backup the original files)
 2. Make mydns323_2009 directory at system root:
    mkdir mydns323_2009
 3. Add below lines in file etc/rc.local:
     mount <fix IP of DNS-323>:/mydns323_2009 /mydns323_2009
 4. Make sure file etc/resolv.conf has content:
        nameserver <fix IP of DNS-323>
        search local <your domain e.g. usmgrid.myren.net.my or ignore this line>
 5. Reboot the machine (Your Fedora)
```
