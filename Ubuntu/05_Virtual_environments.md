   - [Task](#task)
   - [Miniconda virtual environment](#miniconda)
      - [Install Miniconda](#install-miniconda)
      - [Set up and configure Miniconda virtual environment](#configure-miniconda)
      - [Install TensorFlow for GPU](#tensorflow-gpu)
   - [Anaconda virtual environment](#anaconda)
      - [Install Anaconda](#install-anaconda)
      - [Set up and configure Anaconda virtual environment](#configure-anaconda)
   - [Common virtual environment](#venv)
      - [Install packages for virtual environment](#install-packages)
      - [Set up and configure virtual environment](#configure-venv)
         - [Virtual environment for Ubuntu 20.04](#venv_20.04)
         - [Virtual environment for Ubuntu 18.04](#venv_18.04)
   - [EasyBuild environment on SURFsara server](#easy-build)

---
### <a name="task" />Task

Install software for virtual environments. Set up and configure virtual env.

---
### <a name="miniconda" />Miniconda virtual environment


#### <a name="install-miniconda" />Install Miniconda


#### <a name="configure-miniconda" />Set up and configure Miniconda virtual environment


#### <a name="tensorflow-gpu" />Install TensorFlow for GPU


---
### <a name="anaconda" />Anaconda virtual environment

I recommend Anaconda virtual environment, but you could use common virtual env
described in the next chapter.

Links:
   - [How to Install Anaconda on Ubuntu 20.04](https://tecnstuff.net/how-to-install-anaconda-on-ubuntu-20-04/)
   - [How To Install Anaconda on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart)
   - [Anaconda distribution](https://www.anaconda.com/distribution)
   - [Anaconda repository](https://repo.anaconda.com/archive)

#### <a name="install-anaconda" />Install Anaconda

```shell script
# Download latest Anaconda distribution
mkdir -p ~/Documents/Install/Anaconda/
cd ~/Documents/Install/Anaconda
curl -O https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh

# Run installation script
bash Anaconda3-2020.02-Linux-x86_64.sh

# Next, you will be prompted to download Visual Studio Code,
# which you can learn more about from the official VSCode website.
# Type 'no' to decline, if you're not sudo user.

# Activate Installation
source ~/.bashrc
# Test installation
conda list

# Update Anaconda
conda update conda
conda update anaconda

# Uninstall Anaconda
#conda install anaconda-clean
#anaconda-clean ––yes
```

#### <a name="configure-anaconda" />Set up and configure Anaconda virtual environment

```shell script
# To activate installed Anaconda use command:
source ~/.bashrc

# Set up Anaconda virtual environment
conda create --name myenv python=3.8
# Show virtual envs
conda info --envs
# Activate the new environment
conda activate myenv

# After activation of virtual environment,
# install TensorFlow using Anaconda
conda install tensorflow-gpu
# Check it
python -c "import tensorflow as tf;     \
    print('Version:', tf.__version__);  \
    print(tf.reduce_sum(tf.random.normal([1000, 1000])));"

# Activate and deactivate virtual environment
conda deactivate  # exit to the "base" environment
conda deactivate  # exit from Anaconda base env
conda activate myenv

# Install necessary packages:
conda install tensorflow-gpu matplotlib scipy opencv pillow \
              scikit-learn scikit-image pandas ipython \
              ipyparallel jupyter pyyaml graphviz -n myenv

# Install PyTorch if necessary.
# Check https://pytorch.org for installation parameters
conda install pytorch torchvision cudatoolkit=10.2 -c pytorch

# Delete vitrual environment if necessary
conda deactivate
conda remove --name myenv --all
conda info --envs
```

---
### <a name="venv" />Common virtual environment

Links:
   - [Python Virtual Environment](https://www.geeksforgeeks.org/python-virtual-environment)
   - [Pipenv & Virtual Environments](https://docs.python-guide.org/dev/virtualenvs)

#### <a name="install-packages" />Install packages for virtual environment

```shell script
# Python virtual environment creator
sudo apt install virtualenv
sudo apt install python3-virtualenv

# Library for generating Python executable zip files
sudo apt install pex
sudo apt install python3-pex

# Node.js virtual environment builder
sudo apt install nodeenv

# Script for cloning a non-relocatable virtualenv
sudo apt install python3-virtualenv-clone

# Extension to virtualenv for managing multiple virtual Python environments
sudo apt install virtualenvwrapper

# apt virtual environment
sudo apt install apt-venv

# pyvenv-3 binary for python 3.x
sudo apt install python3-venv
```

How-to install `virtualenvwrapper` [for Windows 10](https://pypi.org/project/virtualenvwrapper-win):

```shell script
rem For TensorFlow you have to install
rem Python >= 3.7.2 (64-bit); CUDA >= 10.0; cuDNN >= 7.5
:
set PATH=c:\path-to-your\Python37\Scripts\;c:\path-to-your\Python37\;%PATH%
: In my case: set PATH=c:\Programs\Python37\Scripts\;c:\Programs\Python37\;%PATH%
echo %PATH%
pip3 --version
pip3 install virtualenvwrapper-win
```

It is better to use virtual environment.

```shell script
# user installation
# pip install --user packagename

# Install Pipenv
pip install --user pipenv
```

#### <a name="configure-venv" />Set up and configure virtual environment

##### <a name="venv_20.04" />Virtual environment for Ubuntu 20.04

**NOTE**: For Ubuntu 20.04 setting up a virtual environment is different:
[How To Install Python 3 and Set Up a Programming Environment on an Ubuntu 20.04 Server](https://www.digitalocean.com/community/tutorials/how-to-install-python-3-and-set-up-a-programming-environment-on-an-ubuntu-20-04-server)

Install Python 3.7 for Ubuntu **20.04** (build from source):
```shell script
sudo apt update  
sudo apt upgrade -y
sudo apt install build-essential libssl-dev zlib1g-dev \
                 libncurses5-dev libncursesw5-dev libreadline-dev \
                 libsqlite3-dev libgdbm-dev libdb5.3-dev libbz2-dev \
                 libexpat1-dev liblzma-dev tk-dev libffi-dev
cd ~/Downloads/
wget https://www.python.org/ftp/python/3.7.8/Python-3.7.8.tar.xz
tar xf Python-3.7.8.tar.xz
cd Python-3.7.8
./configure --enable-optimizations
sudo make -j 8
sudo make altinstall
# Check it
python3.7

# Create virtual environment for Python 3.7 for Ubuntu 20.04
cd ~
python3.7 -m venv python3.7
source ~/python3.7/bin/activate
```

##### <a name="venv_18.04" />Virtual environment for Ubuntu 18.04

Install virtual environment for Ubuntu **18.04**:
```shell script
# Check version of virtual environment
virtualenv --version
# We'll use virtualenvwrapper — is a set of extensions to Ian Bicking's virtualenv tool.
virtualenvwrapper

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

# Install TensorFlow 2.0 alpha0 — make sure you need this version of TF.
pip install -U --pre tensorflow-gpu
# or
pip install tensorflow-gpu

# Check it
python -c "import tensorflow as tf;     \
    print('Version:', tf.__version__);  \
    print(tf.reduce_sum(tf.random.normal([1000, 1000])));"

# Install all other packages into myenv
conda install tensorflow-gpu matplotlib scipy opencv pillow \
              scikit-learn scikit-image pandas ipython \
              ipyparallel jupyter pyyaml graphviz

# Install PyTorch if necessary
# NOTE: check your installation here: https://pytorch.org/
pip install torch torchvision

# Deactivate myenv
deactivate

# Delete virtual environment if you don't need it any more.
rmvirtualenv myenv
```

---
### <a name="easy-build" />EasyBuild environment on SURFsara server

Links to read:
   * [Tutorial: Easybuild and Environment Modules](https://varrette.gforge.uni.lu/blog/2017/06/01/tutorial-easybuild/)
   * [EasyBuild documentation](https://easybuild.readthedocs.io/en/latest)

Notes:
   - You can login to SURFsara only from DeepLab3, because its IP address is in the whitelist.
   - [SURFsara documentation page](https://userinfo.surfsara.nl/systems/cartesius/usage/batch-usage) --
   You can try to use the search functionality of userinfo as not everything is referenced, but is findable.
   - Use `accinfo` command or [`portal.surfsara.nl`](https://portal.surfsara.nl) to track hours remained.
   - If you have problems interacting with the batch environment please send a message at
   `helpdesk <at> surfsara <dot> nl`.
   - If you have any ML framework/application setup, cluster behavior, etc. you can email me directly
   `"Damian Podareanu" <damian <dot> podareanu <at> surfsara <dot> nl>` and just CC
   `helpdesk <at> surfsara <dot> nl`.
   - `nvidia-smi` command doesn't work.
   - Operating system: Red Hat Enterprise Linux Server 7.6 (Maipo). Command to check: `hostnamectl`
   - 17 TB of free space in $HOME directory. Command to check: `df -h ~/`
   - 16 CPU cores and 126 GB RAM. Command to check: `htop`

You can install local software modules in EasyBuild environment
or use [Anaconda virtual environment](#anaconda) from previous chapter.

```shell script
module --help    # Get help about Modules package
man module       # Get manual about Modules package
module avail     # List all the modules which are available to be loaded.
module list      # List loaded modules

module avail | grep -i conda  # search for 'conda' packages
module av | grep -i python    # search for 'python' packages
module av python              # search for 'python' - case sensitive
module available Python       # search for 'Python' - case sensitive

module load eb   # Load EasyBuild framework
eb -S Anaconda   # List Anaconda packages
eb -S MiniConda  # List Miniconda packages
```

Install and configure Anaconda:

```shell script
# Install Anaconda package. Wait for 5 minutes.
module load eb   # Load EasyBuild framework
eblocalinstall Anaconda3-5.3.0.eb --robot

# Load Anaconda module. To load Anaconda 5.3.0 you must exit and login again.
# Otherwise you can load only older version Anaconda 5.0.1.
exit  # exit from SURFsara and login again to activate Anaconda3 5.3.0
module load Anaconda3/5.3.0
#module load Anaconda3/5.0.1

# Check which Anaconda is used
which anaconda
conda --version
# Set up 'test' Anaconda virtual environment
conda create --name test python=3
# Show virtual environments
conda info --envs
# Activate 'test' environment
source activate test

# Install necessary packages with one line in activated 'test' environment
conda install tensorflow-gpu matplotlib scipy opencv pillow \
              scikit-learn scikit-image pandas ipython \
              ipyparallel jupyter pyyaml graphviz -n test

# Update Anaconda if necessary
conda update -n base -c defaults conda
# If you have inconsistency problem, run
conda install anaconda

# Deactivate 'test' environment
source deactivate

# Delete 'test' vitrual environment if necessary
source deactivate
conda remove --name test --all
conda info --envs
```

Install and configure Python:

```shell script
# There are several Python versions installed already
module av Python
# Load Python module
module load Python/3.6.3-foss-2017b
# Load cuDNN module, so TensorFlow can be imported
module load cuDNN/7.3.1-CUDA-10.0.130
# Create 'test' virtual environment
virtualenv test
# Activate 'test' virtual environment
source ~/test/bin/activate
# Check which Python is used
which python
python --version
# Start Python in the local environment
python3

# Install necessary packages with one line in activated 'test' environment
pip install tensorflow-gpu matplotlib scipy opencv pillow \
              scikit-learn scikit-image pandas ipython \
              ipyparallel jupyter pyyaml graphviz

# Deactivate 'test' environment
deactivate

# Delete 'test' virtual environment if you don't need it any more.
rmvirtualenv test
```

To transfer files via `gsiscp` and `gsisftp` read
[this instruction](14_Install_GSI_openssh_client.md/#manage).
