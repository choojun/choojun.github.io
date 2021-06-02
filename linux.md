
### Ubuntu 20.04
```
 sudo apt-get install openssh-server wget gimp youtube-dl vlc dia istanbul recordmydesktop patch firefox vim r-base subversion 
 sudo apt-get install ngspice latex2rtf texmaker latex2rtf texlive-science texlive-* 
 sudo apt-get install unace unrar zip unzip p7zip p7zip-full p7zip-rar sharutils unrar rar uudeview mpack arj cabextract file-roller mencoder flac faac faad sox ffmpeg2theora libmpeg2-4 uudeview mpeg3-utils mpegdemux mpeg2dec vorbis-tools id3v2 mpg321 mpg123 icedax lame libmad0 libjpeg-progs libdvdread4 libdvdnav4 imagemagick dos2unix

 # For development use
 sudo apt-get install mysql-server mysql-client mysql-workbench
 # follow instructions here for the change of password https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04
 sudo apt-get install git
 sudo apt-get install maven

 For Java development use
 1. Extract the bin / gz file:
 tar -xzvf jdk-8u251-linux-x64.tar.gz
 2. Move extracted folder to this location:
 sudo mkdir -p /usr/lib/jvm
 sudo mv ./jdk1.8.0_251 /usr/lib/jvm/
 3. Install new java source in system:
 sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_251/bin/javac 1
 sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_251/bin/java 1
 sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.8.0_251/bin/javaws 1
 4. Suppose that you have multiple version of Java, you may choose default java via commands:
 sudo update-alternatives --config javac
 sudo update-alternatives --config java
 sudo update-alternatives --config javaws
 5. To know your version test:
 java -version
 javac -version
 javaws -version
 5. Enable Java in a web browser (Google Chrome and Mozilla Firefox) on Ubuntu Linux, remore more at URL https://java.com/en/download/help/enable_browser_ubuntu.xml
 For Mozilla Firefox
 a. Create a directory called plugins if you do not have it. 
 sudo mkdir -p /usr/lib/firefox-addons/plugins
 b. Go to Mozilla plugins directory before you make the symbolic link.
 sudo cd /usr/lib/firefox-addons/plugins
 c. Create a symbolic link
 sudo ln -s /usr/lib/jvm/jdk1.8.0_251/jre/lib/amd64/libnpjp2.so
 d. Restart your browser and test it with URL https://java.com/en/download/testjava.jsp
```

### Ubuntu 18.04
```
 sudo apt-get install openssh-server wget gimp youtube-dl vlc dia istanbul gtk-recordmydesktop patch firefox vim r-base subversion 
 sudo apt-get install ngspice latex2rtf texmaker latex2rtf texlive-science texlive-* 
 sudo apt-get install unace unrar zip unzip p7zip p7zip-full p7zip-rar sharutils unrar rar uudeview mpack arj cabextract file-roller mencoder flac faac faad sox ffmpeg2theora libmpeg2-4 uudeview mpeg3-utils mpegdemux mpeg2dec vorbis-tools id3v2 mpg321 mpg123 icedax lame libmad0 libjpeg-progs libdvdread4 libdvdnav4 imagemagick dos2unix

 # For development use
 sudo apt-get install mysql-server mysql-client mysql-workbench
 # follow instructions here for the change of password https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04
 sudo apt-get install git
 sudo apt-get install maven

```


### Java Development Kit 1.8 and Ubuntu 16.04
```
 sudo add-apt-repository ppa:webupd8team/java
 sudo apt-get update
 sudo apt install oracle-java8-installer
 
 or
 
 1. Make the bin file executeable:
 chmod +x jdk-6u32-linux-x64.bin
 2. Extract the bin / gz file:
 ./jdk-6u32-linux-x64.bin
 tar -xzvf jdk-6u32-linux-x64.tar.gz
 3. Move extracted folder to this location:
 sudo mv jdk1.6.0_32 /usr/lib/jvm/
 or
 sudo mkdir -p /usr/lib/jvm
 sudo mv ./jdk1.6.0_32 /usr/lib/jvm/
 4. Install new java source in system:
 sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.6.0_32/bin/javac 1
 sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.6.0_32/bin/java 1
 sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.6.0_32/bin/javaws 1
 sudo update-alternatives --install /usr/lib/mozilla/plugins/mozilla-javaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.6.0_32/jre/lib/amd64/libnpjp2.so 1
 
 sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_221/bin/javac 1
 sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_221/bin/java 1
 sudo update-alternatives --install /usr/bin/javaws javaws /usr/lib/jvm/jdk1.8.0_221/bin/javaws 1
 sudo update-alternatives --install /usr/lib/mozilla/plugins/mozilla-javaplugin.so mozilla-javaplugin.so /usr/lib/jvm/jdk1.8.0_221/jre/lib/amd64/libnpjp2.so 1
 5. Choose default java:
 sudo update-alternatives --config javac
 sudo update-alternatives --config java
 sudo update-alternatives --config javaws
 6. Java version test:
 java -version
 7. Verify the symlinks all point to the new java location:
 ls -la /etc/alternatives/java*
 8. Web site for verification
 http://www.java.com/en/download/installed.jsp?detect=jre
```

### Maven3 and Ubuntu 16.04
```
sudo apt-get purge maven maven2 maven3
sudo apt-add-repository ppa:andrei-pozolotin/maven3
sudo apt-get update
sudo apt-get install maven3
```


### Ubuntu 16.04
```
sudo apt-get update
sudo apt-get install apache2
  sudo apache2ctl configtest
  sudo nano /etc/apache2/apache2.conf 
  Inside, at the bottom of the file, add a ServerName directive, pointing to your primary domain name. If you do not have a domain name associated with your server, you can use your server's public IP address
  sudo apache2ctl configtest
  sudo ufw app list
  sudo ufw allow in "Apache Full"
  sudo ufw app info "Apache Full"
  http://your_server_IP_address 
  You will see the default Ubuntu Apache web page, which is there for informational and testing purposes.

sudo apt-get install mysql-server
  During the installation, your server will ask you to select and confirm a password for the MySQL "root" user. This is an administrative account in MySQL that has increased privileges. Think of it as being similar to the root account for the server itself (the one you are configuring now is a MySQL-specific account, however). Make sure this is a strong, unique password, and do not leave it blank.
  When the installation is complete, we want to run a simple security script that will remove some dangerous defaults and lock down access to our database system a little bit. Start the interactive script by running
  mysql_secure_installation
  You will be asked to enter the password you set for the MySQL root account. Next, you will be asked if you want to configure the VALIDATE PASSWORD PLUGIN. Warning: Enabling this feature is something of a judgment call. If enabled, passwords which don't match the specified criteria will be rejected by MySQL with an error. This will cause issues if you use a weak password in conjunction with software which automatically configures MySQL user credentials, such as the Ubuntu packages for phpMyAdmin. It is safe to leave validation disabled, but you should always use strong, unique passwords for database credentials. Answer y for yes, or anything else to continue without enabling. You'll be asked to select a level of password validation. Keep in mind that if you enter 2, for the strongest level, you will receive errors when attempting to set any password which does not contain numbers, upper and lowercase letters, and special characters, or which is based on common dictionary words.
  For the rest of the questions, you should press Y and hit the Enter key at each prompt. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes we have made.

sudo apt-get install php libapache2-mod-php php-mcrypt php-mysql
  In most cases, we'll want to modify the way that Apache serves files when a directory is requested. Currently, if a user requests a directory from the server, Apache will first look for a file called index.html. We want to tell our web server to prefer PHP files, so we'll make Apache look for an index.php file first. To do this, type this command to open the dir.conf file in a text editor with root privileges:
  sudo nano /etc/apache2/mods-enabled/dir.conf
  <IfModule mod_dir.c>
    DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
  </IfModule>
  <IfModule mod_dir.c>
    DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
  </IfModule>

sudo systemctl restart apache2
sudo systemctl status apache2

sudo apt-get install php-cli

sudo apt-get install furiusisomount

gsettings set org.gnome.nautilus.preferences default-folder-viewer 'list-view'
```


### Ubuntu 16.04 and nVidia

First completely uninstall the NVIDIA drivers which you have tried to install in your attempt before.

Start the laptop, mark the Ubuntu entry in the GRUB boot menu and then press the E key.
Add nouveau.modeset=0 at the end of the linux line. Press the F10 key to boot the system.
Do not miss to set a Space between the last letter in the linux line and nouveau.modeset=0.

When the login screen appears press Ctrl+Alt+F1. Enter user name and password - execute :
```
sudo apt purge nvidia*
sudo reboot  
```
Now install the latest stable NVIDIA drivers 381.22 (or check your required version at URL http://www.nvidia.com/Download/index.aspx) and nvidia-primefrom the GPU Drivers PPA.

After the restart mark the Ubuntu entry in the GRUB boot menu again and press the E key.
Add nouveau.modeset=0 at the end of the linux line. Press the F10 key to boot the system.
Do not miss to set a Space between the last letter in the linux line and nouveau.modeset=0.

When the login screen appears press Ctrl+Alt+F1. Enter user name and password - execute :
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo apt install nvidia-378 nvidia-prime
sudo reboot  
```
Execute lspci -k | grep -EA2 'VGA|3D' ... now you'd see : Kernel driver in use: nvidia

In case you still have problems to get the NVIDIA drivers working, you should consider to opt-in to the Ubuntu LTS enablement stacks, which provide newer kernel and X support for existing Ubuntu LTS releases. This could generally be a good idea, because you are having a quite new notebook. Opt-in to the Ubuntu 16.04 LTS - HWE - enablement stacks by executing the following command :
```
sudo apt install --install-recommends linux-generic-hwe-16.04 xserver-xorg-hwe-16.04  
sudo reboot  
```
Before you perform it, remove all NVIDIA software as described in step 1 and reinstall the drivers as described in step 2 after you have installed the new kernel and rebooted the operating system.

Additional information : Boot into the BIOS to make sure that Secure Boot is disabled and that the NVIDIA graphics chip and NVIDIA Optimus are enabled (in some machines this option is available).



### AMD Redeon driver and Linux
* ATI's site http://support.amd.com/us/gpudownload/Pages/index.aspx
* Then once you have downloaded the driver open the folder that it is in.
* Then open the terminal and type sudo nautilus you may need to type in your password.
* Next drag the file in the root folder.
* Right click in the file then select permissions next click on the make executable box.
* Now double click on the file select run and follow the steps.
* DO NOT ACTIVATE THE DRIVERS IN SYSTEM<ADMINISTRATION<HARDWARE DRIVERS! It will messes up the other driver you just installed.




### Ubuntu 14.04
```
 xxx sudo apt-get install cdbs fakeroot dh-make dkms wget execstack libelfg0 module-assistant openssh-server mplayer mplayer-gui gecko-mediaplayer mencoder gimp youtube-dl vlc dia istanbul gtk-recordmydesktop latex2rtf texmaker latex2rtf texlive-science texlive-* gparted ffmpeg smplayer p7zip unrar patch firefox thunderbird ia32-libs gnome-session-flashback vim r-base subversion
 xxx sudo apt-get install cdbs fakeroot dh-make dkms execstack libelfg0 module-assistant thunderbird gparted

 sudo apt-get install openssh-server wget gimp youtube-dl vlc dia istanbul gtk-recordmydesktop patch firefox vim r-base subversion 
 sudo apt-get install ngspice latex2rtf texmaker latex2rtf texlive-science texlive-* 
 sudo apt-get install unace unrar zip unzip p7zip p7zip-full p7zip-rar sharutils unrar rar uudeview mpack arj cabextract file-roller libxine1-ffmpeg mencoder flac faac faad sox ffmpeg2theora libmpeg2-4 uudeview libmpeg3-1 mpeg3-utils mpegdemux mpeg2dec vorbis-tools id3v2 mpg321 mpg123 libflac++6 totem-mozilla icedax lame libmad0 libjpeg-progs libdvdread4 libdvdnav4 imagemagick 

 # For development use
 sudo apt-get install mysql-server
 sudo apt-get install git
 sudo apt-get install --no-install-recommends install maven2

 # For Eclipse update
 http://www.eclipse.org/downloads/packages/release/indigo/sr2

 # For Gnome GUI (tested in 12.04)
 gsettings set org.gnome.desktop.interface ubuntu-overlay-scrollbars false
 gsettings set com.canonical.indicator.session show-real-name-on-panel true

 # For Gnome GUI (tested in 14.04)
 gsettings set com.canonical.desktop.interface scrollbar-mode normal
 sudo apt-get install gnome-session-flashback

 # Requirements for HOTBIT 0.1 & ASE 3.5.2
 sudo apt-get install python-dev python-matplotlib python-scipy python-numpy libblas liblapack libblas-* liblapack-* libatlas-* mpich2 netcdf-bin fftw-dev gfortran python-scientific grace rasmol gnuplot python-pexpect python-vtk python-matplotlib python-gnuplot 

 # Requirements for DFTB+ (wou / j*p7yae3 / http://www.dftb-plus.info)
 sudo apt-get install gfortran gawk jmol openjdk-7-jdk make liblapack-* libblas-* cpp python python-dev python-matplotlib python-scipy python-numpy libblas-* liblapack-* libatlas-* python-vtk python-matplotlib python-gnuplot netcdf-bin fftw-dev gfortran python-scientific grace rasmol gnuplot python-pexpect

 # For nice GUI (tested in 12.04)
 sudo apt-get install cairo-dock cairo-dock-plug-ins
```

Thinkpad W530, Nvidia Discrete Graphics driver, and Ubuntu
```
 # Enabling external monitor on Lenovo W530 with Nvidia Discrete Graphics and Ubuntu 12.04 (non-nVidia driver)
 # http://blog.pearce.org.nz/2012/08/enabling-external-monitor-on-lenovo.html
 # Or this http://www.nvidia.com/object/unix.html
 1) Go to BIOS. Set Graphics Device to nVidia Optimus and OS auto detection to Disable. Virtualization turning to Disable.
 2) Install Ubuntu or what ever you prefer.
 3) activate the Ubuntu partner repository and do a full dist-upgrade of your system. Reboot
 4) atm there is no repository for quantal!!! So I have installed the nvidia-current form ubuntu repo.
 sudo add-apt-repository ppa:ubuntu-x-swat/x-updates
 sudo apt-get update
 sudo apt-get install nvidia-current 
 5) Create the /usr/share/X11/xorg.conf.d/10-nvidia-brightness.conf and fill whith the content like shown above. (If I hadn't this, my system was freezing on pressing [Fn]+[F8/F9])
 sudo gedit /usr/share/X11/xorg.conf.d/10-nvidia-brightness.conf
 Paste the following into the file:
    Section "Device"
        Identifier     "Device0"
        Driver         "nvidia"
        VendorName     "NVIDIA Corporation"
        BoardName      "Quadro K2000M"
        Option         "RegistryDwords" "EnableBrightnessControl=1"
    EndSection
 6) Edit the /etc/default/grub and set the nox2apic as default boot parameter. Do update-grub.
    GRUB_CMDLINE_LINUX_DEFAULT="nox2apic quiet splash"
 7) Go to BIOS. Change Graphics Device to Discrete. Os auto detection still stays at Disable! Now you can turn on the Virtualization. Safe all settings and boot in you Ubuntu.
 Note: Often deleting (or renaming if you don't want to delete) your ~/.gnome2/ or ~/.gconf/, or some other ~/.$config/ directories enables Ubuntu to re-init its settings and reconfigure correctly.
 Now everything should work with multi-monitor.

 #Revert back to open source nouveau (driver for nvidia), check the supported nVidia card at http://www.nvidia.com/object/unix.html
 sudo apt-get remove --purge nvidia-*
 sudo rm /etc/X11/xorg.conf #if file does not exit is OK# 
 sudo apt-get install nvidia-common ubuntu-desktop
 sudo apt-get install --reinstall xserver-xorg-video-nouveau
 sudo dpkg-reconfigure xserver-xorg
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


### Adobe Flash Plugin in Ubuntu

```
 # 32bit
 sudo apt-get install flashplugin-installer
 # 64bit
 sudo apt-get install flash64plugin-installer
```



### Ubuntu 12.04
```
Logitech's Unifying mouse or keyboard

 sudo add-apt-repository ppa:daniel.pavel/solaar
 sudo apt-get update
 sudo apt-get install solaar solaar-gnome3

#For basic

 sudo apt-get install cdbs fakeroot dh-make dkms wget execstack libelfg0 module-assistant openssh-server mplayer mplayer-gui gecko-mediaplayer mencoder gimp youtube-dl vlc dia istanbul gtk-recordmydesktop latex2rtf texmaker latex2rtf texlive-science texlive-* gparted ffmpeg smplayer p7zip unrar patch firefox thunderbird ia32-libs gnome-session-fallback vim r-base subversion
```


### UBuntu 11.10
```
 sudo apt-get install build-essential cdbs fakeroot dh-make debhelper debconf libstdc++6 dkms libqtgui4 wget execstack libelfg0 module-assistant ia32-libs subversion phython-dev openssh-server mplayer mplayer-gui gecko-mediaplayer mencoder vlc dia istanbul gtk-recordmydesktop latex2rtf texmaker latex2rtf texlive-science texlive-* gparted ffmpeg  gtkpod smplayer p7zip unrar libblas-dev liblapack-dev
 sudo add-apt-repository ppa:ferramroberto/java
 sudo apt-get update
 sudo apt-get install sun-java6-jdk
 sudo ufw enable
 sudo allow 22
 MPICH2 in UBuntu https://help.ubuntu.com/community/MpichCluster
```


### UBuntu 11.04
```
 sudo add-apt-repository "deb http://archive.canonical.com/ lucid partner"
 sudo apt-get update
 sudo apt-get install sun-java6-jdk
 sudo apt-get install firefox thunderbird  mplayer mplayer-gui gecko-mediaplayer mencoder vlc dia istanbul gtk-recordmydesktop latex2rtf texmaker latex2rtf texlive-science texlive-* gparted ffmpeg  gtkpod smplayer p7zip unrar
 more at http://cinderbox.net/2011/04/03/to-do-list-after-installing-ubuntu-11-04-aka-natty-narwhal/
 sudo ufw enable
 sudo allow 22
```






### Fedora 12
```
 sudo yum install yum-plugin-fastestmirror
 sudo rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
 sudo rpm -ivh http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm
 sudo yum update firefox thunderbird pidgin gimp mysql mysql-* openoffice.org-* 
 yum install VirtualBox-OSE VirtualBox-OSE-guest kmod-VirtualBox-OSE
 # Download codec http://www.mplayerhq.hu/MPlayer/releases/codecs/all-20100303.tar.bz2
 sudo mkdir -p /usr/lib/codecs
 sudo tar -jxvf all-20100303.tar.bz2 --strip-components 1 -C /usr/lib/codecs/
 # for DVD playing
 sudo rpm -ivh http://rpm.livna.org/livna-release.rpm
 sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-livna
 sudo yum install libdvdcss
 sudo yum install xmms xmms-mp3 xmms-faad2 xmms-pulse xmms-skins rhythmbox gstreamer gstreamer-plugins-ugly gstreamer-plugins-bad gstreamer-ffmpeg amarok xine-lib-extras-freeworld mplayer mplayer-gui gecko-mediaplayer mencoder xine-ui xine-lib-extras xine-lib-extras-freeworld vlc dia istanbul gtk-recordmydesktop latex2rtf texmaker latex2rtf texlive-science texlive-* gparted ffmpeg mysql-query-browser mysql-administrator gtkpod smplayer p7zip unrar
 sudo yum install audacious audacious-plugins-freeworld*
 # for MS font, download http://www.mjmwired.net/resources/files/msttcore-fonts-2.0-3.noarch.rpm
 sudo rpm -ivh msttcore-fonts-2.0-3.noarch.rpm
 # more guide  at http://www.mjmwired.net/resources/mjm-fedora-f12.html http://www.dnmouse.org/guides.html http://www.if-not-true-then-false.com/
 # or use Autoten 
 # For F11:
 # sudo rpm --import http://dnmouse.org/RPM-GPG-KEY-dnmouse
 # sudo rpm -Uvh http://dnmouse.org/autoten-4.1-8.fc11.noarch.rpm
 # For F10:
 # sudo rpm --import http://dnmouse.org/RPM-GPG-KEY-dnmouse
 # sudo rpm -Uvh  http://dnmouse.org/autoten-4-1.fc11.noarch.rpm 
```


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





