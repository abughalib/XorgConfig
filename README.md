# Xorg Config
Xorg configuration file for Intel and Nvidia driver dual display.
Tested in: <br>
Debian 10<br>
Kali Linux 2021.1<br>
Ubuntu 20.10<br>

## Situation
This configuration file is for those who wants to use external **monitor to a laptop** having **Intel GPU** for internal display and **NVIDIA as discrete GPU** which outputs through HDMI or DP.

## Installation
* **Nvidia Driver**<br>

1. Debian and its derivatives<br>
```bash
apt update
```
```bash
apt install nvidia-driver
```

**or**<br>
Install from https://www.nvidia.com/en-in/drivers/unix/ <br>

Download **Latest Long Lived Branch** currently its 450.80.02<br>
To install nvidia from .run file you would need to install linux-header and build essential.

```bash
sudo apt install linux-headers-$(uname -r) build-essential gcc make g++
```
You may required nvidia-kernel-dkms, if below code doesn't work search on internet for it.<br>
Or search apt search nvidia-*
```bash
sudo apt install nvidia-kernel-dkms
```
Restart your PC, At login screen press Ctrl+Alt+F2 or Ctrl+Alt+F3 and login as root there<br>
```bash
cd /home/$USER/Downloads/
```
Install driver i.e
```bash
./NVIDIA-Linux-x86_64-450.80.02.run
```
**If the above doesn't work search on internet to install nvidia driver.**<br>
Install nvidia-xconfig
```bash
apt install nvidia-xconfig
```
Generate xorg.conf using
```
nvidia-xconfig
```
and restart your pc, you would get your external display working but laptop display would be blank or stuck on Modern Display Manger.<br>
Move this xorg.conf to /etc/X11/
```bash
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
```bash
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
```bash
reboot
```
## Output on 2nd Display
**To get Providers name run**<br>
```bash
xrandr --listproviders
```<br>
Output maybe something like this<br>
```
Providers: number : 2
Provider 0: id: 0x1b8 cap: 0x1, Source Output crtcs: 4 outputs: 1 associated providers: 1 name:NVIDIA-0
Provider 1: id: 0x1fa cap: 0x2, Sink Output crtcs: 3 outputs: 1 associated providers: 1 name:modesetting

```
**To output on secondary display run**<br>
*making sure that you're not running wayland if you're not sure use any other desktop environment other then Gnome*
```bash
xrandr --setprovideroutputsource modesetting NVIDIA-0
```
**To make sure xrandr automatically runs**<br>
*Don't use systemd to autostart a script like that it would be very tough to make it work that way*<br>
Recommended Desktop Login Manager is SDDM (Plasma) but you can use any, only advantage being it outputs in both displays
since login page but others after login... as per my tests<br>
* Optional if sddm is install <br>
```bash
dpkg-reconfigure sddm
```
**Auto start using SDDM**<br>
Add these to the end of file:<br>
```bash
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto
```
```bash
nano /usr/share/sddm/scripts/Xsetup
```
*ctrl+o && ctrl+x  to save and exit*<br>
**Auto start using GDM3**<br>
Add these before exit 0 in <br>
```bash
xrandr --setprovideroutputsource modesetting NVIDIA-0
xrandr --auto

```
```bash
nano /etc/gdm3/Init/Default
```

### Fix tearing on Intel Display

```bash
su root
```
```bash
mkdir -p /etc/X11/xorg.conf.d
mv 20-intel.conf /etc/X11/xorg.conf.d/
```
Reboot
```
reboot
```
### Fix display position
Setting->Display drag screen the way you like it.
