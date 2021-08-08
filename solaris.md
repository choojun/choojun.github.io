Install VLC in OpenSolaris 2009.06 
```
 # download http://blastwave.network.com/csw/pkgutil_i386.pkg, and install it
 pkgadd -d pkgutil_i386.pkg
 mkdir /etc/opt/csw   &&  cp -p /opt/csw/etc/pkgutil.conf.CSW /etc/opt/csw/pkgutil.conf
 /opt/csw/bin/pkgutil –catalog
 # install VLC
 /opt/csw/bin/pkgutil –install vlc
```
