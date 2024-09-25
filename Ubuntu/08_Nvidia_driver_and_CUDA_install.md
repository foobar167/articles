:exclamation: **NOTE**: for Ubuntu 20.04 NVIDIA drivers
are installed *by default*. Just check it by `nvidia-smi` command.

   - [Task](#task)
   - [Related links](#links)
   - [NVIDIA driver installation](#driver)
      - [nvidia-smi error](#nvidia-smi-error)
   - [Install CUDA library for all users](#cuda)

---
### <a name="task" />Task
   - Install graphics driver for Palit GeForce GTX 1080 Ti GameRock 11GB GDDR5X [NEB108TT15LC-1020G].
   - Install CUDA library for all users on Ubuntu 18.04.

My answer on AskUbuntu.com: https://askubuntu.com/a/1097211/672237

---
### <a name="links" />Related links
   - [How to Install Nvidia CUDA Toolkit on Ubuntu 18.04 LTS](https://www.howtoforge.com/tutorial/how-to-install-nvidia-cuda-on-ubuntu-1804)
   - [How to install CUDA 9.2 on Ubuntu 18.04](https://www.pugetsystems.com/labs/hpc/How-to-install-CUDA-9-2-on-Ubuntu-18-04-1184)
   - [NVIDIA CUDA Installation Guide for Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html)
   - [How to install Nvidia driver in ubuntu 18.04](https://askubuntu.com/a/1056128/672237)
   - [Commands for NVIDIA install on Ubuntu 16.04](http://christopher5106.github.io/nvidia/2016/12/30/commands-nvidia-install-ubuntu-16-04.html)

---
### <a name="driver" />NVIDIA driver installation

Go to [NVIDIA web-site](https://www.nvidia.com/Download/index.aspx)
and obtain the latest driver for your GPU. In my case it is:

```text
Product Type: GeForce
Product Series: GeForce 10 Series
Product: GeForce GTX 1080 Ti
Operating System: Linux 64-bit
Language: English (US)
```

Press `<SEARCH>` button and check that the founded driver is
supporting your GPU model in "SUPPORTED PRODUCTS" tab.

Download it. In my case the filename is: `NVIDIA-Linux-x86_64-410.78.run`

```shell script
# Change permission to run and execute it
chmod +x NVIDIA-Linux-x86_64-410.78.run

# Before installation install gcc and make packages:
sudo apt install gcc
sudo apt install make
```

It is better to run driver installation in the text mode.
For the text mode press 3 keys `<Ctrl>+<Alt>+<F3>` and login to console.

Most likely you'll have problems with previously installed graphics driver
called *Nouveau*.

```shell script
# Stop graphical interface
sudo service lightdm stop

# If /tmp directory is too small
df -h /tmp
# Increase /tmp size to 10GB.
sudo umount /tmp
sudo mount -t tmpfs -o size=10485760000,mode=1777 overflow /tmp

# Remove Nouveau driver
sudo apt purge remove xserver-xorg-video-nouveau

# Remove previously installed NVIDIA driver
sudo apt purge nvidia*
sudo apt autoremove
sudo apt autoclean
sudo rm -rf /usr/local/cuda*

# Execute file and answer the questions during installation
sudo ./NVIDIA-Linux-x86_64-410.93.run
# Continue installation
# Install NVIDIA's 32-bit compatibility libraries? — Yes
# Whould you like to run the nvidia-xconfig utility... — Yes
# OK

# Start graphical interface
sudo service lightdm start

# Reboot Ubuntu
sudo reboot

# After reboot check if installation is successful
nvidia-smi
```

You should see terminal output of Nvidia Drivers:

![Output of `nvidia-smi` command](data/2018.11.29_check_nvidia_driver.png)

More checks

```shell script
# Check again
lsmod | grep nouveau  # should be zero output
lsmod | grep nvidia   # should be non-zero output

# Another check. {tab} means you should press <Tab> button on your keyboard.
cat /proc/driver/nvidia/gpus/{tab}/information

cat /proc/driver/nvidia/gpus/0000\:17\:00.0/information
Model:           GeForce GTX 1080 Ti
IRQ:             63
GPU UUID:        GPU-e953c3c5-dba7-029e-ce44-7b51417131df
Video BIOS:      86.02.40.00.48
Bus Type:        PCIe
DMA Size:        47 bits
DMA Mask:        0x7fffffffffff
Bus Location:    0000:17:00.0
Device Minor:    0
Blacklisted:     No

cat /proc/driver/nvidia/gpus/0000\:65\:00.0/information
Model:           GeForce GTX TITAN X
IRQ:             64
GPU UUID:        GPU-4ce175fe-a313-4c4a-d620-560e96e8ef2e
Video BIOS:      84.00.45.00.03
Bus Type:        PCIe
DMA Size:        40 bits
DMA Mask:        0xffffffffff
Bus Location:    0000:65:00.0
Device Minor:    1
Blacklisted:     No
```

You should see correct model of your GPU:

![NVIDIA GPU information](data/2018.11.29_info_about_driver.png)

---
### <a name="nvidia-smi-error" />nvidia-smi error

Error when executing command `nvidia-smi`:
`NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver.
Make sure that the latest NVIDIA driver is installed and running.`

_Try to not mix runfile and package manager install methods._

Package manager method ([link](http://maxprog.net.pl/artifficial-intelligence-in-practice/solved-deep-learning-on-gpu-and-error-nvidia-smi-has-failed-because-it-couldnt-communicate-with-the-nvidia-driver)):
```shell script
# Install ubuntu-drivers
# sudo apt install ubuntu-drivers-common
# Check what driver is valid
sudo ubuntu-drivers devices
```
![sudo ubuntu-drivers devices](data/2019.09.19_ubuntu-drivers_devices.png)
```shell script
# Pick the right driver and run it
sudo apt install nvidia-driver-430
# Sometimes --fix-missing flag is needed
# sudo apt install nvidia-driver-535 --fix-missing
# Install CUDA toolkit
sudo apt install nvidia-cuda-toolkit
```

Runfile method:
```shell script
# Go to directory with nVidia driver
cd ~/Documents/Install/nVidia Geforce GTX 1080 Ti

# Download graphical driver from nVidia web site and make it executable.
chmod +x NVIDIA-Linux-x86_64-418.43.run

# Install necessary packages
sudo apt install gcc make

# Press <Ctrl>+<Alt>+<F3> and login to console.
# Stop graphical interface
sudo service lightdm stop

sudo apt autoremove
sudo apt autoclean

# Execute file and answer the questions during installation
sudo ./NVIDIA-Linux-x86_64-410.93.run

# Start graphical interface
sudo service lightdm start

# Reboot Ubuntu
sudo reboot

# After reboot check if installation is successful
nvidia-smi
```

Reinstall [`nvidia-docker`](old/10_Neural_networks_software.md/#container) if necessary.

Troubleshooting:
```shell script
# Remove Nouveau drivers
apt search nouveau  # check if all nouveau drivers are uninstalled
sudo apt purge xserver-xorg-video-nouveau

# You can remove previously installed NVIDIA driver
# sudo apt purge nvidia*  # there are nvidia-docker packages there
```

---
### <a name="cuda" />Install CUDA library for all users

```shell script
# Update and upgrade your system
sudo apt update
sudo apt upgrade

# Verify you have a CUDA-capable GPU
lspci | grep -i nvidia

# Verify you have a supported version of Linux
# x86_64 line indicates you are running on a 64-bit system
# which is supported by cuda 10.0
uname -m && cat /etc/*release

# View linux kernel header
uname -r  # somethins like — 4.15.0-39-generic

# Install gcc, kernel headers and development libraries
sudo apt install gcc-6  # gcc compiler
sudo apt install g++-6  # g++ compiler
sudo apt install linux-headers-$(uname -r)  # kernel headers
sudo apt install freeglut3-dev libxmu-dev libpcap-dev  # development libs
```

To download CUDA toolkit open [NVIDIA web-site](https://developer.nvidia.com/cuda-downloads)

Select: `Linux` → `x86_64` → `Ubuntu` → `18.04` → `runfile (local)`.

Download 2.0 GB file: `cuda_10.0.130_410.48_linux.run`

```shell script
sudo service lightdm stop  # Stop graphical interface

# Change permissions and run it
sudo chmod +x cuda_10.0.130_410.48_linux.run
sudo ./cuda_10.0.130_410.48_linux.run

Do you accept the previously read EULA?
accept/decline/quit: accept

Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 410.48?
(y)es/(n)o/(q)uit: yes

Do you want to install the OpenGL libraries?
(y)es/(n)o/(q)uit [ default is yes ]: yes

Do you want to run nvidia-xconfig?
This will update the system X configuration file so that the NVIDIA X driver
is used. The pre-existing X configuration file will be backed up.
This option should not be used on systems that require a custom
X configuration, such as systems with multiple GPU vendors.
(y)es/(n)o/(q)uit [ default is no ]: no

Note: I think there should be "no" and I donot know why :-)

Install the CUDA 10.0 Toolkit?
(y)es/(n)o/(q)uit: yes

Enter Toolkit Location
 [ default is /usr/local/cuda-10.0 ]: /usr/local/cuda-10.0

Do you want to install a symbolic link at /usr/local/cuda?
(y)es/(n)o/(q)uit: yes

Install the CUDA 10.0 Samples?
(y)es/(n)o/(q)uit: yes

Enter CUDA Samples Location
 [ default is /home/lab225 ]: /home/lab225/Documents/Samples/CUDA-10.0_Samples

sudo service lightdm start  # Start graphical interface
```

If installation is successful, your should see the following output:

```shell script

===========
= Summary =
===========

Driver:   Installed
Toolkit:  Installed in /usr/local/cuda-10.0
Samples:  Installed in /home/lab225/Documents/Samples/CUDA-10.0_Samples

Please make sure that
 -   PATH includes /usr/local/cuda-10.0/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-10.0/lib64, or, add /usr/local/cuda-10.0/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run the uninstall script in /usr/local/cuda-10.0/bin
To uninstall the NVIDIA Driver, run nvidia-uninstall

Please see CUDA_Installation_Guide_Linux.pdf in /usr/local/cuda-10.0/doc/pdf for detailed information on setting up CUDA.

Logfile is /tmp/cuda_install_8756.log
```

To configure the CUDA environment for all users (and applications) on your
system create two files (use `sudo` and a text editor of your choice)

```shell script
# Create file cuda.sh
sudo touch /etc/profile.d/cuda.sh
# Open cuda.sh file
sudo nano /etc/profile.d/cuda.sh
# Add content to the cuda.sh file
export PATH=$PATH:/usr/local/cuda/bin
export CUDADIR=/usr/local/cuda

# Also create file cuda.conf
sudo touch /etc/ld.so.conf.d/cuda.conf
# Open cuda.conf file
sudo nano /etc/ld.so.conf.d/cuda.conf
# Add content to the file
/usr/local/cuda/lib64

# Restart dynamic linker run-time bindings
sudo ldconfig

# Create symbolic links to GCC6 in the CUDA bin folder.
sudo ln -s /usr/bin/gcc-6 /usr/local/cuda-10.0/bin/gcc
sudo ln -s /usr/bin/g++-6 /usr/local/cuda-10.0/bin/g++

# Test CUDA by building the examples
# Copy the CUDA samples source directory to someplace in your home directory
# Go to the directory with the samples and run:
make -j4  # build all examples using 4 CPU processes

# There could be compilation error for the samples
# Error: cannot find -lGL
# Fix it by following the instructions in this link (final 2 commands):
# http://techtidings.blogspot.com/2012/01/problem-with-libglso-on-64-bit-ubuntu.html
sudo rm /usr/lib/x86_64-linux-gnu/libGL.so
sudo ln -s /usr/lib/libGL.so.1 /usr/lib/x86_64-linux-gnu/libGL.so

# Verify CUDA version
nvcc --version  # 10.0 or higher
cat /usr/local/cuda/version.txt
```
