# Xorg Config
Xorg configuration file for Intel and Nvidia driver dual display

## Situation
This configuration file is for those who wants to use external **monitor to a laptop** having **Intel GPU** for internal display and **NVIDIA as discrete GPU** which outputs through HDMI or DP.

## Installation
* **Nvidia Driver**<br>

1. Debian and its derivatives<br>
```
apt update
```
```
apt install nvidia-driver
```

**or**<br>
Install from https://www.nvidia.com/en-in/drivers/unix/ <br>

Download **Latest Long Lived Branch** currently its 450.80.02<br>
To install nvidia from .run file you would need to install linux-header and build essential.

```
sudo apt install linux-headers-$(uname -r) build-essential gcc make g++
```
You may required nvidia-kernel-dkms, if below code doesn't work search on internet for it.<br>
Or search apt search nvidia-*
```
sudo apt install nvidia-kernel-dkms
```
Restart your PC, At login screen press Ctrl+Alt+F2 or Ctrl+Alt+F3 and login as root there<br>
```
cd /home/$USER/Downloads/
```
Install driver i.e
```
./NVIDIA-Linux-x86_64-450.80.02.run
```
**If the above doesn't work search on internet to install nvidia driver.**<br>
Install nvidia-xconfig
```
apt install nvidia-xconfig
```
Generate xorg.conf using
```
nvidia-xconfig
```
and restart your pc, you would get your external display working but laptop display would be blank or stuck on Modern Display Manger.<br>
Move this xorg.conf to /etc/X11/
```
git clone https://github.com/abughalib/XorgConfig
cd XorgConfig
```
### Configure xconf file according to your output.
**Get Bus ID**
```
lscpi | grep VGA
```
Output maybe something like this<br>
```
00:02.0 VGA compatible controller: Intel Corporation UHD Graphics xxx (Mobile)
01:00.0 VGA compatible controller: NVIDIA Corporation GP107M [GeForce GTX XXXX YY Mobile]...
```
Now edit te xorg.conf using any text editor in my case I used nano.
```
nano xorg.conf
```
Replace the BUS ID
like shown for Nvidia and intel
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BusID          "PCI:1:0:0"
    Option         "AllowEmptyInitialConfiguration"
EndSection

Section "Device"
    Identifier     "intel"
    Driver         "modesetting"
    Option         "AccelMethod" "sna"
    Option         "TearFree" "true"
    Option         "TripleBuffer" "true"
    BusID          "PCI:0:2:0"
EndSection
```
Hit Ctrl+s to save and Ctrl+x to exit the editor, now go root and move this file as instructed below.<br>
```
su root
```
Enter your root password, if root not configured use ```sudo -i```<br>
```
mv xorg.conf /etc/X11/
```
reboot
```
reboot
```
### Fix tearing on Intel Display

```
su root
```
```
mkdir -p /etc/X11/xorg.conf.d
mv 20-intel.conf /etc/X11/xorg.conf.d/
```
Reboot
```
reboot
```
### Fix display position
Setting->Display drag screen the way you like it.
