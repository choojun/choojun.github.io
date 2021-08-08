It was tested with Fedora 11 and a USB thumb drive.
```
 yum install -y livecd-tools
 mkdir /media/iso
 livecd-iso-to-disk –reset-mbr <path to>/Fedora-11-i386-DVD.iso  /dev/sdb1
 mkdir /media/<usb disk>/images
 mount -o loop <path to>/Fedora-11-i386-DVD.iso /media/iso
 cp /media/iso/images/install.img  /media/<usb disk>/images/
 cp <path to>/Fedora-11-i386-DVD.iso /media/<usb disk>/
```
