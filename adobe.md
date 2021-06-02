### Adobe Acrobat Reader in Fedora

```
sudo rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-i386-1.0-1.noarch.rpm
sudo rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-x86_64-1.0-1.noarch.rpm
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux
sudo yum install AdobeReader_enu
```

### Adobe Flash Plugin in Fedora

```
 sudo rpm -ivh http://linuxdownload.adobe.com/adobe-release/adobe-release-i386-1.0-1.noarch.rpm
 sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-adobe-linux
 sudo yum install flash-plugin
```

### Adobe Flash Plugin in Ubuntu

```
 # 32bit
 sudo apt-get install flashplugin-installer
 # 64bit
 sudo apt-get install flash64plugin-installer
```

### Adobe Air in Ubuntu 14.04

Install i386 libraries, that are required for successful installation and running of Adobe Air and air applications 
```
 sudo apt-get install libxt6:i386 libnspr4-0d:i386 libgtk2.0-0:i386 libstdc++6:i386 libnss3-1d:i386 lib32nss-mdns libxml2:i386 libxslt1.1:i386 libcanberra-gtk-module:i386 gtk2-engines-murrine:i386
```

Install libgnome-keyring0:i386 package 
```
 sudo apt-get install libgnome-keyring0:i386
```

Create symlinks to gnome-keyring so Adobe Air could see it 
````
 sudo ln -s /usr/lib/x86_64-linux-gnu/libgnome-keyring.so.0 /usr/lib/libgnome-keyring.so.0
 sudo ln -s /usr/lib/x86_64-linux-gnu/libgnome-keyring.so.0.2.0 /usr/lib/libgnome-keyring.so.0.2.0
```

Download Adobe Air installer from http://airdownload.adobe.com/air/lin/download/2.6/AdobeAIRInstaller.bin

Give execute permission and then run that .bin file 
```
 sudo chmod +x AdobeAIRInstaller.bin
 sudo ./AdobeAIRInstaller.bin
```

Courtesy of http://www.tkalin.com/blog_posts/installing-adobe-air-and-elance-tracker-on-ubuntu-13-10-saucy-salamander-64-bit 
