 
## Using Integrated Graphics for Display 
---
Main referenece link: https://gist.github.com/alexlee-gk/76a409f62a53883971a18a11af93241b 

**Disclaimer**: This tutorial/instruction was created for **Ubuntu 18.04** using an **Intel** CPU and a dedicated **NVidia** graphics card. 

### **Step 1**: Find out the Bus ID for the graphic cards you have available. 
``` 
lspci | egrep 'VGA|3D'
```
Running the above command in your terminal should return something similar to the following output: 
```
00:02.0 VGA compatible controller: Intel Corporation Device 3e98
01:00.0 VGA compatible controller: NVIDIA Corporation Device 1f02 (rev a1)
```

### **Step 2**: Make sure that you have the appropriate drivers. 

You'll only really have to worry about installing the graphics drivers for your dedicated graphics cards. As of Ubuntu 18.04, intel has said that the appropriate drivers for intel integrated will automatically be provided in the Ubuntu installations and updates. However, if you have AMD integrated, that's a different story entirely. At the time that this manual is being written, AMD graphics drivers (both integrated and dedicated) have to manually updated. 

### **Step 3**: Configure xorg.conf
You will need to modify the `xorg.conf` file. On my machine, it is to be created in the following directory: 

```
/etc/X11/
```

I suspect it will be the same or similar for you. 

The following is a sample config for you to infer how to select specific screens to be run by a different graphics card:

```
Section "ServerLayout"
    Identifier "layout"
    Screen 0 "intel"
    Screen 1 "nvidia"
EndSection

Section "Device"
    Identifier "intel"
    Driver "intel"
    BusID "PCI:0@00:02:0"
    Option "AccelMethod" "SNA"
EndSection

Section "Screen"
    Identifier "intel"
    Device "intel"
EndSection

Section "Device"
    Identifier "nvidia"
    Driver "nvidia"
    BusID "PCI:1@0:0:0"
    Option "ConstrainCursor" "off"
EndSection

Section "Screen"
    Identifier "nvidia"
    Device "nvidia"
    Option "AllowEmptyInitialConfiguration" "on"
    Option "IgnoreDisplayDevices" "CRT"
EndSection
```
The config provided above hasn't been tested (taken from link referenced above) in it's exact state, but it **should** work (assuming you have the Bus ID set correctly). 

My exact config used looks like the following: 
```
Section "ServerLayout" 
    Identifier "layout"
    Screen 0 "intel"
EndSection 

Section "Device"
    Identifier "intel"
    Driver "intel"
    BusID "PCI:0@00:02:0"
    Option "AccelMethod" "SNA"
EndSection 

Section "Screen"
    Identifier "intel" 
    Device "intel"
EndSection 
```
My purpose was to only run my single monitor using my integrated graphics only so that the dedicated NVidia graphics can be used for cuda calculations (i.e. TensorFlow training and evaluation)