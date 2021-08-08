### Install Realplayer inside 64-bit Linux
```
 # need following requirement at Ubuntu
 sudo apt-get install pax redhat-lsb
 # for Fedora, download real player rpm from http://www.real.com/linux and install the dependency module
 sudo yum install compat-libstdc++-33
 # Install your downloaded rpm, then (for example)
 sudo rpm -hiv RealPlayer10GOLD.rpm
 # In case you do have Helix Player installed, remove it. Not worth it to me.
 # to see helix already installed,
 sudo rpm -qa | grep HelixPlayer
 # If installed, remove it with
 sudo yum remove HelixPlayer
```

Useful links:

* http://mattaustin.me.uk/2009/04/realplayer-on-ubuntu-810-intrepid-64-bit-machines-amd64/
* https://help.ubuntu.com/community/RealPlayerInstallationMethods
* http://www.real.com/realplayer/download
