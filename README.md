# Xorg Config
Xorg configuration file for Intel and Nvidia driver dual display

## Situation
This configuration file is for those who who want to use external **monitor to a laptop** having **Intel GPU** for internal display and **NVIDIA as discrete GPU** which outputs through HDMI or DP.

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
./NVIDIA-Linux-x86_64-450.66.run 
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
and restart your pc, you would get your external display working but laptop display won't.<br>
Move this xorg.conf to /etc/X11/
```
git clone https://github.com/abughalib/XorgConfig
cd XorgConfig
su root
```
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
