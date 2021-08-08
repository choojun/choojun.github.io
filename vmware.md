
```
ESXi 6.0.2 0H43H-4UK0Q-K8781-0A3U4-38Y34
```

### To launch VMware Player VMs without GUI

After the installation and suppose that the VM was created using VMware Player, to start a particular VM :
```
 $ vmrun -T player start /path/to/vm/my.vmx nogui
```
To reboot VM:
```
 $ vmrun -T player reset /path/to/vm/my.vmx soft
```
To power off VM:
```
 $ vmrun -T player stop /path/to/vm/my.vmx soft
```
Suppose that the VM was created using VMware Workstation or VMware Fusion, you can take a snapshot of a running VM as follows
```
 $ vmrun -T ws (or fusion) snapshot /path/to/vm/my.vmx my_snapshot
```



### Patch for VMware workstation 6.5.3 at Fedora 11 (2.6.29 and 2.6.30 kernel) 
```
 mv /usr/bin/gcc /usr/bin/gcc.disable
 # using installer rpm or sh
 # some may need kernel-headers gcc gcc-c++ kernel-devel
 mv /usr/bin/gcc.disable /usr/bin/gcc
```
    
In VMware, you may experience some problems with arrow keys, pg up/pg down and home/end keys with vmware 6.5 and Ubuntu Linux Intrepid 8.10. A workaround for those problems is to create a file named config under .vmware folder in your home directory and fill it with either (both workarounds work!): 
```
 a) 
 xkeymap.nokeycodeMap = true
 b) 
 xkeymap.keycode.108 = 0x138 # Alt_R
 xkeymap.keycode.106 = 0x135 # KP_Divide
 xkeymap.keycode.104 = 0x11c # KP_Enter
 xkeymap.keycode.111 = 0x148 # Up
 xkeymap.keycode.116 = 0x150 # Down
 xkeymap.keycode.113 = 0x14b # Left
 xkeymap.keycode.114 = 0x14d # Right
 xkeymap.keycode.105 = 0x11d # Control_R
 xkeymap.keycode.118 = 0x152 # Insert
 xkeymap.keycode.119 = 0x153 # Delete
 xkeymap.keycode.110 = 0x147 # Home
 xkeymap.keycode.115 = 0x14f # End
 xkeymap.keycode.112 = 0x149 # Prior
 xkeymap.keycode.117 = 0x151 # Next
 xkeymap.keycode.78 = 0x46 # Scroll_Lock
 xkeymap.keycode.127 = 0x100 # Pause
 xkeymap.keycode.133 = 0x15b # Meta_L
 xkeymap.keycode.134 = 0x15c # Meta_R
 xkeymap.keycode.135 = 0x15d # Menu
```

