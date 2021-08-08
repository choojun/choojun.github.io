Install mplayer,mencoder into your linux. Create a file, using the given name, with the following lines. They were tested in Fedora 9.

### avi2mpg
```
 usage="usage: avi2mpg <avi_path>"
 if [ $# = 0 ]
 then
 /bin/echo 1>&2 "$usage"
 exit 1
 fi
 base=`echo "$1" | sed -e 's/\.avi$//' -e 's/\.AVI$//'`
 mencoder -oac copy -ovc lavc -lavcopts vcodec=mpeg4 -o $base.mpg $1
```

### ogg2mpg 
```
 usage="usage: ogg2mpg <ogg_path>"
 if [ $# = 0 ]
 then
 /bin/echo 1>&2 "$usage"
 exit 1
 fi
 base=`echo "$1" | sed -e 's/\.ogg$//' -e 's/\.OGG$//'`
 mencoder $1 -o $base.avi -oac mp3lame -ovc lavc
 mencoder -oac copy -ovc lavc -lavcopts vcodec=mpeg4 -o $base.mpg $base.avi
 rm $base.avi
```

### ogg2rmvb 
Note that Helix Producer can be found here https://helix-producer.helixcommunity.org/Downloads and uncompress the downloaded Helix Producer to local. Your avi maximum size only allow 2Gb. 
```
 usage="usage: ogg2rmvb <ogg_path>"
 if [ $# = 0 ]
 then
 /bin/echo 1>&2 "$usage"
 exit 1
 fi
 base=`echo "$1" | sed -e 's/\.ogg$//' -e 's/\.OGG$//'`
 mencoder $1 -o $base.avi -ovc raw -oac pcm
 ./producer/producer -i $base.avi -o $base.rmvb
 rm $base.avi
```


### flec2mp3 
```
 for FILE in *.flac
 do
 echo Converting $FILE
 # Decode FLAC back to WAV
 /usr/bin/flac -d "$FILE"
 # Save the filename without extension
 BASE=`basename "$FILE" .flac`
 # Convert resulting WAV to MP3 with variable bitrate preserving good quality
 /usr/bin/lame -h -V 2 "$BASE".wav "$BASE".mp3
 done
```
