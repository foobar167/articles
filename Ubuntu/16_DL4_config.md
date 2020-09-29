Configurations I've made on DL4 server for **Ubuntu 20.04**.

   - [How-to configure TensorFlow](#tf-how-to)
   - [Install useful software](#software)
   - [`pyenv` virtual environment](#pyenv)
   - [Shows PIDs of NVIDIA processes](#permissions)

---
## <a name="tf-how-to" />How-to configure TensorFlow
You must have CUDA Toolkit and CuDNN installed to use TensorFlow.
Use virtual environment to install CUDA Toolkit and CuDNN.
```shell script
# Install Anaconda into virtual environment
pyenv install anaconda3-2020.02
# Activate virtual environment
pyenv local anaconda3-2020.02
# Install TF for GPU + installation of CUDA Toolkit and CuDNN inside
conda install tensorflow-gpu
# Enter into Iron Python
ipython
# Import TensorFlow
import tensorflow as tf
# Check available GPU
tf.test.is_gpu_available()
```

---
## <a name="software" />Install useful software

```shell script
sudo apt install -y htop  # CPU monitoring
sudo apt install -y git  # Git
sudo apt install -y mc  # Midnight Commander
sudo apt install -y autoconf  # automatic configure script builder
sudo apt install -y make  # utility for directing compilation
sudo apt install -y curl  # tool for transferring data with URL syntax
sudo apt install -y gcc g++  # GCC and C++ compilers
sudo apt install -y net-tools  # ifconfig command
sudo apt install -y traceroute  # traceroute command
sudo apt install -y emacs  # Emacs editor
sudo apt install -y filezilla  # FileZilla client
sudo apt install -y geeqie geeqie-common  # Geeqie image viewer
```

Install [OpenSlide](https://openslide.org/download/):
```shell script
sudo apt install openslide-tools
# Install Python interface into virtual environment
pip install openslide-python
```

Unfortunately, installations via `snap` do not work.
The error is:

```text
2020/07/19 14:53:45.767662 cmd_run.go:563: WARNING: XAUTHORITY environment value is not a clean path: "/share/home/<username>/.Xauthority"
cannot perform operation: mount --rbind /home /tmp/snap.rootfs_UCBqUu//home: Permission denied
```

```shell script
# Install Notepad++ editor
sudo apt install -y snapd            # install Snap tool
sudo apt install -y wine-stable      # install Wine tool
sudo snap install notepad-plus-plus  # install Notepad++

sudo snap install chromium  # Chromium web browser

sudo snap install pycharm-community --classic  # PyCharm IDE
# Remove possible error with Canberra Gtk.
# canberra-gtk-module translates GTK+ widgets signals to event sounds
# This module is needed for PyCharm successful start
sudo apt install -y libcanberra-gtk-module
```

---
## <a name="pyenv" />`pyenv` virtual environment
Links:
  - [Common build problems](https://github.com/pyenv/pyenv/wiki/common-build-problems)
  - [`pyenv` github](https://github.com/pyenv/pyenv)
  - [`pyenv` automatic installer](https://github.com/pyenv/pyenv-installer)
  - [Managing Multiple Python Versions With `pyenv`](https://realpython.com/intro-to-pyenv/)

```shell script
# Install required libraries
sudo apt install -y build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git

sudo apt install -y libedit-dev

# The automatic installer
curl https://pyenv.run | bash

nano ~/.bashrc  # edit .bashrc file

# Load pyenv automatically by adding
# the following to the end of ~/.bashrc file:
export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

exec $SHELL  # restart shell
pyenv update  # update
```

[Configure](https://realpython.com/intro-to-pyenv/)
Python virtual environment using `pyenv`.

```shell script
pyenv install --list  # show all available versions

# Choose Python version and install it.
# This will take a while because `pyenv`
# is building Python from source.
pyenv install -v 3.8.4

# Uninstall Python version if necessary
#rm -rf ~/.pyenv/versions/3.8.4
# or
#pyenv uninstall 3.8.4

# Set Python version
pyenv versions  # show installed versions
pyenv local 3.8.4  # set an application-specific version
#pyenv global 3.8.4  # use Python 3.8.4 globally
python --version  # get current Python version
#python -m test  # run the built-in test suite
#pyenv commands  # list of pyenv commands

# Create virtual environment
# pyenv virtualenv <python_version> <environment_name>
pyenv virtualenv 3.8.4 myproject
pyenv local myproject  # activate environment

# Deactivation doesn't work without default system Python.
# Just create another virtual environment and activate it.
pyenv deactivate  # deactivate environment
#pyenv virtualenv-delete myproject  # delete venv
```

For example, install Anaconda:

```shell script
pyenv install anaconda3-2020.02
pyenv local anaconda3-2020.02
pyenv virtualenv anaconda3-2020.02 myproject2
pyenv local myproject2  # activate environment
conda --version  # check it
```

---
## <a name="permissions" />Shows PIDs of NVIDIA processes

Create and edit file with `visudo` editor
in the directory `/etc/sudoers.d/`.

```shell script
# Create and edit file
sudo visudo -f /etc/sudoers.d/show_nvidia_pid

# Write these lines into the file /etc/sudoers.d/show_nvidia_pid

# Members of the bioimage and genomics groups
# can view NVIDIA processes
User_Alias USERS = %bioimage, %genomics

# $USER $HOST = (root) NOPASSWD: /the/absolute/path/to/your/command
USERS ALL=NOPASSWD: /bin/fuser -v /dev/nvidia*
```

For more information about `sudoers.d` see
[allow user permissions](07_Website_software.md#permissions).

```shell script
# Create alias for the command
alias nvidia-pids='sudo fuser -v /dev/nvidia*'

# Or create nvidia-show executable file
echo 'sudo fuser -v /dev/nvidia*' | sudo tee -a /usr/bin/nvidia-show
sudo chmod u=rwx,g=rx,o=rx /usr/bin/nvidia-show
ls -hal /usr/bin/nvidia-show /usr/bin/nvidia-smi  # check it
```
