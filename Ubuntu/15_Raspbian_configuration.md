# Raspbian configuration
Install and confibure [Raspbian](https://www.raspberrypi.org/downloads/raspbian)
Buster with desktop and recommended software.

   - [Remote desktop](#remote-desktop)
   - [Set static IP-address](#static-ip)
   - [Set wi-fi](#wi-fi)
   - [Set configurations](#set-configs)
       - [Enable SSH, VNC, camera, CLI and wi-fi](#raspi-config)
       - [System upgrade](#update-upgrade)
       - [Clean cache](#clean-cache)
       - [Virtual keyboard](#virtual-keyboard)
       - [Change screen resolution](#screen-resolution)
       - [Multiple keyboard layouts](#keyboard-layouts)
   - [Install necessary software](#necessary-software)
   - [Get info](#get-info)
   - [Mount flash disk](#mount-disk)
   - [Video4Linux](#video4linux)
   - [KeDei display](#kedei)

---
## <a name="remote-desktop" />Remote desktop
Install [AnyDesk](http://deb.anydesk.com/howto.html) remote desktop.
Run the following commands as root user:
```shell script
# Add repository key to Trusted software providers list
cd ~/Downloads
wget -qO - https://keys.anydesk.com/repos/DEB-GPG-KEY | sudo apt-key add -
# Add the repository
echo "deb http://deb.anydesk.com/ all main" | sudo tee -a /etc/apt/sources.list.d/anydesk-stable.list > /dev/null
# Update apt cache
sudo apt update
# Install anydesk
sudo apt install anydesk
```
Add alias, password and login to your Raspberry Pi remotely.

Install and configure [Real VNC](https://help.realvnc.com/hc/en-us/articles/360002249917-VNC-Connect-and-Raspberry-Pi)
remote desktop.

---
## <a name="static-ip" />Set static IP-address
Modify the file `/etc/dhcpcd.conf`
```shell script
sudo nano /etc/dhcpcd.conf
```
Add the following text to it
```text
# Ethernet
interface eth0

static ip_address=192.168.1.55/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1

# Wireless
interface wlan0

static ip_address=192.168.1.55/24
static routers=192.168.1.1
static domain_name_servers=192.168.1.1
```
After reboot check the status with the `ifconfig` command

---
## <a name="wi-fi" />Set wi-fi
[Set wi-fi](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
via the command line
```shell script
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```
Modify the file
```text
trl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=BY

network={
        ssid="wifi-name"
        psk="wifi-passwd"
        key_mgmt=WPA-PSK
}
```

---
## <a name="set-configs" />Set configurations

### <a name="raspi-config" />Enable SSH, VNC, camera, CLI and wi-fi
```shell script
# Configuring Raspberry Pi.
# Select "5. Interfacing Options".
# Enable SSH, VNC, camera, CLI and wi-fi.
sudo raspi-config
```
To disable desktop GUI on Raspberry Pi execute command `sudo raspi-config`
and go to menu: `Enable Boot to Desktop...` → `Text console login`.

### <a name="update-upgrade" />System upgrade
Update system's package list. `apt` keeps a list of software sources
on your Pi in a `/etc/apt/sources.list` file.
```shell script
# Update package list
sudo apt update
# Upgrade all packages
sudo apt upgrade
```

### <a name="clean-cache" />Clean cache
```shell script
# Remove unnecessary packages and dependencies
sudo apt autoremove
sudo apt autoclean
# Check the amount of apt-cache on the system
sudo du -sh /var/cache/apt
# Remove cache
sudo apt clean
```
Downloaded package files (.deb files) are kept in `/var/cache/apt/archives`.
You can remove them in order to free up space.

### <a name="virtual-keyboard" />Virtual keyboard
```shell script
sudo apt install matchbox-keyboard
```
Now you can access the keyboard: `Menu` → `Accessories` → `Keyboard`

If the keyboard isn't visible in the menu you can enable it by going to:
`Menu` → `Preferences` → `Main Menu Editor`

### <a name="screen-resolution" />Change screen resolution
Edit file `/boot/config.txt` or go to:
`Menu` → `Preferences` → `Raspberry Pi Configuration` → `System` → `Set Resolution`.
Do not change it from 810×540 resolution if you have KeDei display installed.

### <a name="keyboard-layouts" />Multiple keyboard layouts
There is a quick graphical way to change the keyboard layouts,
the toggle key-combination and have a panel indicator at the same time.
   1. Right click on the panel and choose Add/Remove Panel Items
   2. Click Add
   3. Click 'Keyboard Layout Handler`
   4. Click Close
   5. Right click on the flag that appears on the panel
   6. Choose 'Keyboard Layout Handler Settings'
   7. Uncheck Keep system layouts
   8. Add the layouts you need and change the toggling keycombo to your liking
   9. Smile :-)

---
## <a name="necessary-software" />Install necessary software
```shell script
# Check the Raspbian version
cat /etc/os-release
# Midnight Commander
sudo apt install mc
```

<details>
  <summary>Use virtual environment to install Python packages</summary>

Use [virtual environment](05_Virtual_environments.md) to install Python packages.

```shell script
# Delete wolfram package, because its upgrade is extremely slow
sudo apt purge wolfram-engine
# Install packages for Pillow
#sudo apt install libtiff-dev libjpeg-dev zlib1g-dev zlibc libfreetype6-dev \
#                 liblcms2-dev libwebp-dev tcl-dev tk-dev
sudo apt install libjpeg-dev zlibc
# Install it for TensorFlow - https://magpi.raspberrypi.org/articles/tensorflow-ai-raspberry-pi
sudo apt install libatlas-base-dev
# Install for OpenCV
sudo apt install libjasper-dev

# Install Python packages
sudo apt install python-pip         python2.7           \
                 python3-pip        python3             \
                 python-numpy       python3-numpy       \
                 python-scipy       python3-scipy       \
                 python-matplotlib  python3-matplotlib  \
                 python-sklearn     python3-sklearn     \
                 python-skimage     python3-skimage     \
                 python-opencv      python3-opencv      \
                 python-pandas      python3-pandas      \
                 python-pil         python3-pil         \
                 python-pil.imagetk python3-pil.imagetk \
                 python-psutil      python3-psutil      \
                 python-spur        python3-spur        \
                 cython             cython3             \
                 python-ipython     python3-ipython     \
                 ipython            ipython3            \
                 jupyter            git                 \
                 python-mock        python3-mock        \
                 graphviz                               \
                 python-pydot       python3-pydot       \
                 python-pyparsing   python3-pyparsing   \
                 python-ipykernel   python3-ipykernel   \
                 python-dev         python3-dev         \
                 python-yaml        python3-yaml        \
                 python-setuptools  python3-setuptools
```
</details>

```
Install packages for virtual environment
```shell script
# Python virtual environment creator
sudo apt install virtualenv python-virtualenv python3-virtualenv -y
# Library for generating Python executable zip files
sudo apt install pex python-pex python3-pex -y
# Node.js virtual environment builder
sudo apt install nodeenv -y
# System for automatically handling virtual environments
sudo apt install fades -y
# Wrap and build python packages using virtualenv
sudo apt install dh-virtualenv -y
# Script for cloning a non-relocatable virtualenv
sudo apt install virtualenv-clone -y
# Extension to virtualenv for managing multiple virtual Python environments
sudo apt install virtualenvwrapper -y
# apt virtual environment
sudo apt install apt-venv -y
# pyvenv-3 binary for python 3.x
sudo apt install python3-venv -y
```
Set up and configure virtual environment using `virtualenvwrapper`
```shell script
# Check version of virtual environment
virtualenv --version
# We'll use virtualenvwrapper — is a set of extensions to Ian Bicking's virtualenv tool.
virtualenvwrapper_load

# Create virtual environment for Python 3.x
mkvirtualenv -p /usr/bin/python3 myenv

# Directory called "myenv" is created
ls -hal ~/.virtualenvs/myenv/
# Browse inside of "myenv" dir to view its structure.

# List all virtual environments
lsvirtualenv -b  # breaf mode
# or
lsvirtualenv -l  # long mode

# Activate virtual environment
workon myenv

# You should see "(myenv)" before command prompt
# (myenv) username@hostname:~/path/to/dir$
# Make sure you "workon myenv" and install packages into myenv

# Install TensorFlow
#pip install tensorflow  # version 1.13.1 for 2019.11.06
# or
cd ~/Downloads
wget https://github.com/PINTO0309/Tensorflow-bin/raw/master/tensorflow-2.0.0-cp37-cp37m-linux_armv7l.whl
pip install tensorflow-2.0.0-cp37-cp37m-linux_armv7l.whl

# Check it
python -c "import tensorflow as tf;     \
    print('Version:', tf.__version__);  \
    print(tf.reduce_sum(tf.random.normal([1000, 1000])));"

python -c "import tensorflow as tf;  \
           msg = tf.constant('TensorFlow 2.0 Hello World');  \
           tf.print(msg);"

# Install all other packages into myenv
pip install matplotlib scipy opencv-contrib-python \
            Pillow psutil spur cython scikit-learn scikit-image \
            pandas ipython ipyparallel jupyter pyyaml

# Update all python packages. INSIDE your virtual environment
pip freeze > requirements.txt && pip install --upgrade -r requirements.txt && rm requirements.txt

# Deactivate myenv
deactivate

# Delete virtual environment if you don't need it any more.
#rmvirtualenv myenv
```
Package actions
```shell script
# Install package
sudo apt install package_name -y
# Uninstall package
sudo apt remove package_name
# Completely remove package and its associated config files
sudo apt purge package_name
# Search the archives for a package with a given keyword
apt search package_name
# View more info about a package before installing it 
apt show package_name
```
Configure [virtual environment](05_Virtual_environments.md) for your task.

---
## <a name="get-info" />Get info
Get device information (ip-address,
[memory use](https://learn.adafruit.com/an-illustrated-shell-command-primer/checking-file-space-usage-du-and-df),
etc.) with the following commands:
```shell script
# Network information
ifconfig
# Display the IP addresses associated with the device
hostname -I
# Check free space on the file system
df -h
# Get system usage info. To exit press 'q'.
htop
# free = RAM usage, h = human
free –h
# w = who is logged in
w  # who
# Get Python version
python --version
python3 --version
# Tesk camera
raspistill -o image.jpg
raspivid -o video.h264 -t 10000
# Convert h264 to mp4
sudo apt install gpac
MP4Box -add video.h264 video.mp4
```
To change to console mode press `ctrl+alt+f1`.
To change to graphical mode press `ctrl+alt+f7`.
However don't do this via remote desktop.

To check Python packages both inside [virtual environment](05_Virtual_environments.md)
and outside of it call `ipython` or `ipython3` and copy-paste:
```text
from PIL import ImageTk  # check Pillow
#import tensorflow  # check TensorFlow (inside virtual environment only)
import numpy       # check NumPy
import scipy       # check SciPy
import matplotlib  # check MatPlotLib
import sklearn     # check Scikit-learn
import skimage     # check Scikit-image
import cv2         # check OpenCV
import pandas      # check Pandas
import psutil      # check PsUtil
import spur        # check Spur
import cython      # check Cython
exit()
```

---
## <a name="mount-disk" />Mount flash disk
```shell script
# Create a mount point first
sudo mkdir /mnt/usb
# Whenever you plug a drive into your RPi you do
sudo mount /dev/sdb1 /mnt/usb
```

---
## <a name="video4linux" />Video4Linux
Python manages Raspberry's camera by the way of
[picamera module](https://www.raspberrypi.org/learning/getting-started-with-picamera).
You can use this module also in OpenCV but you have to grab images to `numpy.array`
then map the array to OpenCV Mat.

To use the standard grabbing loop `cv2.VideoCapture(0)` with raspicam
the Video4Linux driver is needed.

Check prerequisites with `sudo raspi-config`:
   - Enable the camera
   - Set large memory for gpu_mem (In `Advance Options` → `Memmory Split` set 128 min)

```shell script
# Install v4l library from repository
sudo apt-get -y install libv4l-dev v4l-utils
# Enable the kernel module
sudo modprobe bcm2835-v4l2
# Test the module
v4l2-ctl --list-devices
# You should receive something like this
#     mmal service 16.1 (platform:bcm2835-v4l2):
#              /dev/video0
# Test: try to grab a single frame and check for the file test.jpg
v4l2-ctl --set-fmt-video=width=800,height=600,pixelformat=3
v4l2-ctl --stream-mmap=3 --stream-count=1 --stream-to=test.jpg
# Info: check all available controls like brightness, contrast,.. with
v4l2-ctl --list-ctrls
```
If all it works well add the module name `bcm2835-v4l2` to the list of modules
loaded at boot time in `/etc/modules-load.d/modules.conf` file.

---
## <a name="kedei" />KeDei display
Install driver for [KeDei Raspberry Pi Display](http://kedei.net/raspberry/raspberry.html)
3.5 inch HDMILCD 18bit version 1.0 2016/11/11. Enable SPI via `sudo raspi-config`.
```shell script
# Download LCD_show_35hdmi.tar.gz file
wget http://kedei.net/raspberry/hdmi/LCD_show_35hdmi.tar.gz
# Unpack it
tar -xvzf LCD_show_35hdmi.tar.gz
# Install LCD35_810_540 driver to get 810×540 display resolution
cd LCD_show_35hdmi
./LCD_backup
sudo ./LCD35_810*540
```
If the monitor is constantly rebooting, it means that it does not have enough energy.
Connect the second power cable to the monitor itself, and the monitor will stop rebooting.

Troubleshooting
```text
If there is a warning:
WARNING **: Error retrieving accessibility bus address:
org.freedesktop.DBus.Error.ServiceUnknown:
The name org.a11y.Bus was not provided by any .service files

To fix this warning install the following package
sudo apt install at-spi2-core
```
