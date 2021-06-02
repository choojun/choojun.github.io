### VI cheat sheet
http://www.lagmonster.org/docs/vi.html

### Update datetime at Ubuntu
```
sudo date -s "$(wget -qSO- --max-redirect=0 google.com 2>&1 | grep Date: | cut -d' ' -f5-8)Z"
```

### Replace colon to dash for files
```
find . -name "*:*" -exec rename 's|:|-|g' {} \;
```

### Delete the finding's result
```
find . -name *.java | xargs rm
```

### Create iso
for target directory
```
mkisofs -RJ -o image.iso /burndirectory/
```
for target CD or DVD in sr0, check your drive number with command: sudo lshw -class disk
```
# check your CD-ROM or DVD-ROM drive's with command (with CD/DVD inserted in the drive)
df -k
# creating the ISO file for entire CD/DVD in the drive
sudo cat /dev/sr0 > /home/mitch/example.iso
```

### Burn iso
check available devices
```
wodim --devices
```
perform the burning process
```
cdrecord -v -dao dev=/dev/scd0 Fedora-13-x86_64-DVD.iso
```

### Mount non-device files
```
 #-t iso9660 means to mount it according to the ISO format. Think of it as TAR for disks.
 #-r is indeed equivalent to -o ro: readonly.
 # -o loop means to mount it as a loop device: a device contained within a device.
 sudo mount -r -t iso9660 -o loop image_name.iso /media/iso/
 file myImage.dmg
 sudo mount -t hfs -o loop myImage.dmg /macdisk
 sudo umount -l /media/iso
```

### Creating ISO Image
#### From Bin/Cue Images 
```
bchunk IMAGE.bin IMAGE.cue IMAGE.iso
```
#### From Nero Images 
```
nrg2iso image.nrg image.iso 
```
#### From DMG image
Use dmg2img, rename the file extension from IMG to ISO, and Brasero will recognise the image and let you to burn the disc. 


### Enable auto-login in Fedora
```
 # at file gdm.conf
 [daemon]
 TimedLoginEnable=true
 TimedLogin=choojun
 TimedLoginDelay=3
 # or at file /etc/gdm/gdm.schemas
     <schema>
       <key>daemon/AutomaticLoginEnable</key>
       <signature>b</signature>
       <default>false</default>
     </schema>
     <schema>
       <key>daemon/AutomaticLogin</key>
       <signature>s</signature>
       <default></default>
     </schema>
     <schema>
       <key>daemon/TimedLoginEnable</key>
       <signature>b</signature>
       <default>true</default>
     </schema>
     <schema>
       <key>daemon/TimedLogin</key>
       <signature>s</signature>
       <default>choojun</default>
     </schema>
     <schema>
       <key>daemon/TimedLoginDelay</key>
       <signature>i</signature>
       <default>3</default>
     </schema>
```

### Cron Job in Linux
```
 # list existing job
 crontab -l
 # edit existing jon
 crontab -e
```

### File uncompressing
```
 tar -cjvf 111.tar.bz2 111/
 tar -czvf 111.tar.gz 111/
 gzip -d 222.gz
 gzip -d 333.Z
 unzip test.zip
```

### File compressing
```
 tar xvf archive_name.tar
 # To create a *.gz compressed file:
 gzip test.txt
 # To create a *.bz2 compressed file:
 bzip2 test.txt
```

### Viewing compressed file
```
 tar tvf archive_name.tar
 # Display compression ratio of the compressed file using gzip -l
 gzip -l *.gz
 # View the contents of *.zip file (Without unzipping it):
 unzip -l jasper.zip
```

### Zombie Process
```
 see all zombie process
 ps aux | awk '{ print $8 " " $2 }' | grep -w Z
 kill all zombie process
 kill -HUP `ps -A -ostat,ppid | grep -e '^[Zz]' | awk '{print $2}'`
```

### Grep command examples
```
 # Search for a given string in a file (case in-sensitive search).
 grep -i "the" demo_file
 # Print the matched line, along with the 3 lines after it.
 grep -A 3 -i "example" demo_text
 # Search for a given string in all files recursively
 grep -r "atithya" *
```

### Find command examples
```
 # Find files using file-name ( case in-sensitve find)
 find -iname "MyCProgram.c"
 # Execute commands on files found by the find command
 find -iname "MyCProgram.c" -exec md5sum {} \;
 # Find all empty files in home directory
 find ~ -empty
```

### SSH command
```
 # Login to remote host
 ssh -l jsmith remotehost.example.com
 # Debug ssh client
 ssh -v -l jsmith remotehost.example.com
 # Display ssh client version
 ssh -V
 # example output: OpenSSH_3.9p1, OpenSSL 0.9.7a Feb 19 2003
```

### sed command
```
 # When you copy a DOS file to Unix, you could find \r\n in the end of each line. This example converts the DOS file format to Unix file format using sed command.
 sed 's/.$//' filename
 # Print file content in reverse order
 sed -n '1!G;h;$p' thegeekstuff.txt
 # Add line number for all non-empty-lines in a file
 sed '/./=' thegeekstuff.txt | sed 'N; s/\n/ /'
```

### awk command
```
 # Remove duplicate lines using awk
 awk '!($0 in array) { array[$0]; print }' temp
 # Print all lines from /etc/passwd that has the same uid and gid
 awk -F ':' '$3==$4' passwd.txt
 # Print only specific field from a file.
 awk '{print $2,$5;}' employee.txt
```

### Sort command
```
 # Sort a file in ascending order
 sort names.txt
 # Sort a file in descending order
 sort -r names.txt
 # Sort passwd file by 3rd field.
 sort -t: -k 3n /etc/passwd | more
```

### export command
```
 # To view oracle related environment variables.
 export | grep ORACLE
 # example output: declare -x ORACLE_BASE="/u01/app/oracle"
 # example output: declare -x ORACLE_HOME="/u01/app/oracle/product/10.2.0"
 # example output: declare -x ORACLE_SID="med"
 # example output: declare -x ORACLE_TERM="xterm"
 
 # To export an environment variable:
 export ORACLE_HOME=/u01/app/oracle/product/10.2.0
```


### xargs command
```
 # Copy all images to external hard-drive
 ls *.jpg | xargs -n1 -i cp {} /external-hard-drive/directory
 # Search all jpg images in the system and archive it.
 find / -name *.jpg -type f -print | xargs tar -cvzf images.tar.gz
 # Download all the URLs mentioned in the url-list.txt file
 cat url-list.txt | xargs wget –c
```

### ls command
```
 # Display filesize in human readable format (e.g. KB, MB etc.,)
 ls -lh
 # example output: -rw-r----- 1 atithya team-dev 8.9M Jun 12 15:27 arch-linux.txt.gz
 
 # Order Files Based on Last Modified Time (In Reverse Order) Using ls -ltr
 ls -ltr
 
 # Visual Classification of Files With Special Characters Using ls -F
 ls -F
```

### cd command
 Use “cd -” to toggle between the last two directories
 
 Use “shopt -s cdspell” to automatically correct mistyped directory names on cd




### Disable IPv6 in Ubuntu 12.04
```
 # add below lines into /etc/sysctl.conf
 net.ipv6.conf.all.disable_ipv6 = 1
 net.ipv6.conf.default.disable_ipv6 = 1
 net.ipv6.conf.lo.disable_ipv6 = 1
 # restart the service
 sudo sysctl -p
```

### Enable the arrow keys, tab-complete in Ubuntu
```
 # Arrow keys, tab-complete not working after user is created using the following command
 # useradd -m admin
 # Fix it by issuing the following command
 sudo chsh -s /bin/bash <username>
```


### Linux in SSD Drive

When you delete a file on an old, magnetic hard drive, the computer simply marks that file as deleted. The file’s data sticks around on the hard drive — that’s why deleted files can be recovered. The computer will eventually overwrite the deleted files when it overwrites their sectors with new data.

Solid-state drives (SSDs) work differently. Whenever you write a file to an SSD, the computer must first erase any data in the sectors it’s writing the data to. It can’t just “overwrite” the sectors in one operation — it must first clear them, then write to the empty sectors.

This means that an SSD will slow down over time. Writing to the SSD’s sectors will be quick the first time. After you delete some files and try to write to it again, it will take longer. With TRIM enabled, the operating system tells the SSD each time it deletes a file. The drive can then erase the sectors containing the file’s contents, so writing to the sectors will be quick in the future.

In other words, if you don’t use TRIM, your SSD will slow down over time. That’s why modern operating systems, including Windows 7+, Mac OS X 10.6.8+, and Android 4.3+ use TRIM. TRIM was implemented in Linux back in December 2008, but Ubuntu isn’t using it by default. The real reason Ubuntu doesn’t TRIM SSDs by default is because the Linux kernel’s implementation of TRIM is slow and results in poor performance in normal use. On Windows 7 and 8, Windows sends a TRIM command each time it deletes a file, telling the drive to immediately delete the bits of the file. Linux supports this when file systems are mounted with the “discard” option. However, Ubuntu — and other distributions — don’t do this by default for performance reasons. To TRIM your SSD on Ubuntu, simply open a terminal and run the following command:
```
 sudo fstrim -v /
```
To check your SSD drive supported TRIM:
```
 sudo hdparm -I /dev/sda | grep "TRIM supported"
```
Source: http://www.howtogeek.com/176978/ubuntu-doesnt-trim-ssds-by-default-why-not-and-how-to-enable-it-yourself/

