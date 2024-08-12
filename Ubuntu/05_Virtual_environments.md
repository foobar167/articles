   - [Task](#task)
   - [Miniconda virtual environment](#miniconda)

      - [Install Miniconda](#install-miniconda)
      - [Configure Miniconda virtual environment](#configure-miniconda)
      - [Install TensorFlow for GPU using Conda installer](#tensorflow-conda)
      - [Install TensorFlow for GPU using PIP installer](#tensorflow-conda-pip)

   - [Miniconda and pip](#miniconda-pip)

   - [(OLD) virtualenvwrapper](#virtualenvwrapper-config)


   - [(OLD) Pyenv Python version manager](#pyenv)

[//]: # (      - [Install Pyenv]&#40;#install-pyenv&#41;)
[//]: # (      - [Configure Pyenv]&#40;#configure-pyenv&#41;)
[//]: # (      - [Install TensorFlow for GPU using PIP installer]&#40;#tensorflow-pyenv&#41;)

   - [(OLD) Anaconda virtual environment](#anaconda)

[//]: # (      - [Install Anaconda]&#40;#install-anaconda&#41;)
[//]: # (      - [Configure Anaconda virtual environment]&#40;#configure-anaconda&#41;)

   - [(OLD) Common virtual environment](#venv)

[//]: # (      - [Install packages for virtual environment]&#40;#install-packages&#41;)
[//]: # (      - [Configure virtual environment]&#40;#configure-venv&#41;)
[//]: # (         - [Virtual environment for Ubuntu 20.04]&#40;#venv_20.04&#41;)
[//]: # (         - [Virtual environment for Ubuntu 18.04]&#40;#venv_18.04&#41;)

   - [(OLD) EasyBuild environment on SURFsara server](#easy-build)


---
### <a name="task" />Task

Install software for Python virtual environments.
Set up and configure virtual envs.

---
### <a name="miniconda" />Miniconda virtual environment

To install TensorFlow, I recommend the Miniconda virtual environment,
but you can use Anaconda or the regular virtual environment described below.
The main difference between Miniconda and Anaconda is that Anaconda comes pre-loaded
with hundreds of packages while Miniconda only includes conda and its dependencie.
This means that Anaconda is larger and more convenient to use, but Miniconda is smaller
and more flexible to install.

Links:
   - [Install TensorFlow with pip](https://www.tensorflow.org/install/pip#linux_1)
   - [How to Install TensorFlow on Ubuntu](https://phoenixnap.com/kb/how-to-install-tensorflow-ubuntu)

```shell script
# Install system packages if necessary using sudo user (admin).
# Python-dev for installing header files for Python extensions.
# PIP package manager. 
sudo apt install python3-dev python3-pip
```

#### <a name="install-miniconda" />Install Miniconda

```shell script
# Download latest Miniconda distribution
mkdir -p ~/Downloads/
cd ~/Downloads/
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Run installation script. When asked to initialize Miniconda, type `yes`.
bash Miniconda3-latest-Linux-x86_64.sh

exec $SHELL  # restart the shell
# Verify the installation
conda --version

# If you'd prefer that conda's base environment not be activated on startup, 
#   set the auto_activate_base parameter to false: 
# conda config --set auto_activate_base false

# Update conda to the latest version
conda update -n base -c defaults conda
conda update --all
```

#### <a name="configure-miniconda" />Configure Miniconda virtual environment

```shell script
# Set up conda virtual environment
conda create --name myenv python=3.11
# Show virtual envs
conda info --envs
# Activate the environment
conda activate myenv

# Activate and deactivate virtual environment
conda deactivate  # exit to the "base" environment
conda deactivate  # exit from Miniconda base env
conda activate myenv

# Delete vitrual environment if necessary
#conda deactivate
#conda remove --name myenv --all
#conda info --envs
```

#### <a name="tensorflow-conda" />Install TensorFlow for GPU using Conda installer

First install the [NVIDIA GPU driver](08_Nvidia_driver_and_CUDA_install.md/#nvidia-smi-error)
if you have not. You can use the `nvidia-smi` command to verify it is installed.

NOTE: Try to not mix PIP and Conda installations together!

```shell script
# Activate virtual environment
conda activate myenv

# Search for CUDA Toolkit available versions.
# The CUDA Toolkit enables GPU-accelerated development.
conda search -c conda-forge cudatoolkit
# Install CUDA Toolkit. Use newer version if necessary.
conda install -c conda-forge cudatoolkit=11.8

# Search for cuDNN library available versions.
# The cuDNN package provides GPU acceleration for deep neural networks (DNN).
conda search -c conda-forge cudnn
# Install cuDNN library. Use newer version if necessary.
conda install -c conda-forge cudnn=8.8

# Configure the system paths to activate when running the virtual environment
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
# Export the paths
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/' > \
        $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh

# Search for TensorFlow GPU available versions.
conda search -c conda-forge tensorflow-gpu
# Install TF with GPU support using Miniconda.
# Do not use pip like in official web-site.
# Try to not mix PIP and Conda installations together.

# Check for TF/CUDA/CUDNN versions here:
#     https://www.tensorflow.org/install/source#gpu
conda install -c conda-forge tensorflow-gpu=2.12

# Verify the GPU setup
python -c "import tensorflow as tf; print('\n' + str(len(tf.config.list_physical_devices('GPU'))) + ' GPU available\n')"

# Install other Python packages
conda install -c conda-forge tensorflow-hub matplotlib scipy numpy opencv pillow \
    scikit-learn scikit-image pandas ipython jupyter tqdm graphviz

# Install PyTorch if necessary. But it's better to set it to the different environment.
# Check https://pytorch.org for installation parameters
#conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia

# Install Curl downloader if necessary
#conda install -c anaconda curl
```
#### <a name="tensorflow-conda-pip" />Install TensorFlow for GPU using PIP installer

For now there is no CUDA Toolkit (`cudatoolkit`) for PIP,
but it is available in the Conda repository. So install `cudatoolkit` with Conda
and then install all other packages with PIP like in
[official tutorial](https://www.tensorflow.org/install/pip).

```shell
conda create --name tf python=3.11
conda info --envs
conda activate tf

# Update the pip package manager
which pip  # check pip location
pip install --upgrade pip

# There is no CUDA Toolkit for PIP. Install it with Conda
conda search -c conda-forge cudatoolkit
conda install -c conda-forge cudatoolkit=11.8

# Install cuDNN and TensorFlow with GPU support
pip install nvidia-cudnn-cu11 tensorflow

# Configure the system paths to activate when running the virtual environment
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> \
    $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib' >> \
    $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh

# Verify the GPU setup
python -c "import tensorflow as tf; print('\n' + str(len(tf.config.list_physical_devices('GPU'))) + ' GPU available\n')"

# Install other Python packages
pip install tensorflow-hub matplotlib scipy numpy opencv-python pillow \
    scikit-learn scikit-image pandas ipython jupyter tqdm graphviz
```




---
### <a name="miniconda-pip" />Miniconda and pip

**Install Miniconda**
```shell script
# Download latest Miniconda distribution
mkdir -p ~/Downloads/
cd ~/Downloads/
curl -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

# Run installation script. When asked to initialize Miniconda, type `yes`.
bash Miniconda3-latest-Linux-x86_64.sh

exec $SHELL  # restart the shell
# Verify the installation
conda --version

# If you'd prefer that conda's base environment not be activated on startup, 
#   set the auto_activate_base parameter to false: 
# conda config --set auto_activate_base false

# Update conda to the latest version
conda update -n base -c defaults conda
conda update --all
```

**Configure Miniconda virtual environment**
```shell script
# Set up conda virtual environment
conda create --name myenv python=3.11
#conda create --name myenv python=3.10
# Show virtual envs
conda info --envs
# Activate the environment
conda activate myenv

# Activate and deactivate virtual environment
conda deactivate  # exit to the "base" environment
conda deactivate  # exit from Miniconda base env
conda activate myenv

# Delete vitrual environment if necessary
#conda deactivate
#conda remove --name myenv --all
#conda info --envs
```

**Install all additional packages via PIP installer**

**This instruction doesn't work for old version of CUDA==11.4**
```shell script
conda activate myenv  # activate virtual environment

pip install --upgrade pip  # upgrade pip
pip --version  # verify pip version
which pip      # verify pip location
which python3  # verify python location

# Install TensorFlow with GPU support
#python3 -m pip install tensorflow==2.12[and-cuda]
python3 -m pip install tensorflow[and-cuda]
# Verify the GPU setup
python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
python3 -c "import tensorflow as tf; print('\n' + str(len(tf.config.list_physical_devices('GPU'))) + ' GPU available\n')"

# Install other packages
pip install tensorflow-hub matplotlib scipy numpy opencv-python \
      pillow scikit-learn scikit-image pandas ipython jupyter tqdm graphviz \
      openpyxl

# View installed packages
pip list | grep tensor
````


---
### <a name="virtualenvwrapper-config" />(OlD) virtualenvwrapper

<details>
  <summary>Show details</summary>

Install for Windows. To use `virtualenvwrapper` you **should have
needed version** of Python 3.x on your computer installed.
For example, to use Python 3.9 virtual environment, install standalone
Python 3.9 on your computer first.
```shell
# Create Python 3.9 virtual environment
# Install Python 3.12. Place it in the PATH.
# Install Python 3.9.
pip install virtualenvwrapper
# mkvirtualenv venv_name -p python_version
mkvirtualenv python39 -p 3.9
workon  # show all virtual envs
workon python39  # switch to Python 3.9
python --version  # check it

pip install tensorflow==2.15.* tensorflow-datasets tensorflow-hub \
            matplotlib scipy numpy opencv-python pillow scikit-learn \
            scikit-image pandas ipython jupyter tqdm graphviz nodejs \
            ipyparallel


```

There is an error when `import tensorflow-datasets as tfds` for Windows and PYthon 3.9.
To fix it in the file `~\Envs\python39\lib\site-packages\tensorflow_datasets\core\shuffle.py`
comment string `#import resource` to
```shell
if os.name == 'posix':
    import resource # pylint: disable=import-error
```

</details>


---
### <a name="pyenv" />(OLD) Pyenv Python version manager

<details>
  <summary>Show details</summary>

Links:
   - [Managing Multiple Python Versions With pyenv](https://realpython.com/intro-to-pyenv/)
     - [How to install 'pyenv' Python version manager on Ubuntu 20.04](https://brain2life.hashnode.dev/how-to-install-pyenv-python-version-manager-on-ubuntu-2004)
     - [How to Install and Use Pyenv in Ubuntu](https://itslinuxfoss.com/install-use-pyenv-ubuntu/)

`pyenv` - is a simple Python version manager tool,
which allows to switch between multiple versions of Python.
You can set local or global system-wide Python versions via the tool.

Install required prerequisite dependencies as a system administrator
```shell
sudo apt update; sudo apt install make build-essential libssl-dev zlib1g-dev \
    libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev \
    xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

#### <a name="install-pyenv" />Install Pyenv

```shell
# Download and execute installation script for local account
curl https://pyenv.run | bash
```

At the end of the run, you should see some text like this:

```text
WARNING: seems you still have not added 'pyenv' to the load path.

# Load pyenv automatically by appending
# the following to ~/.bash_profile if it exists, otherwise
# ~/.profile (for login shells) and
# ~/.bashrc (for interactive shells) :

export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"

# Restart your shell for the changes to take effect.

# Load pyenv-virtualenv automatically by adding
# the following to ~/.bashrc:

eval "$(pyenv virtualenv-init -)"
```

The output will be based on your shell.
But you should follow the instructions to add `pyenv` to your path and
to initialize pyenv/pyenv-virtualenv auto completion.

```shell
nano ~/.bashrc  # edit .bashrc file

# Load pyenv automatically by appending the following to
# ~/.bashrc file:
export PYENV_ROOT="$HOME/.pyenv"
command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

exec $SHELL  # restart the shell
pyenv update  # update pyenv
pyenv --version  # get version
```

To uninstall `pyenv` delete `~/.pyenv` directory and modify `~/.bashrc` file.

```shell
#rm -rf ~/.pyenv
#nano ~/.bashrc  # edit .bashrc file
#exec $SHELL  # restart the shell
```

#### <a name="configure-pyenv" />Configure Pyenv

Try to use only `local` or only `global` command to set `pyenv` version.

NOTE: `global` command doesn't work for me.

```shell
pyenv install --list  # show all available versions

# Choose Python version and install it.
# This will take a while because `pyenv` is building Python from source.
pyenv install -v 3.9.16

# Uninstall Python version if necessary
#rm -rf ~/.pyenv/versions/3.9.16
# or
#pyenv uninstall 3.9.16

# Set Python version
pyenv versions  # show installed versions
pyenv local 3.9.16  # set an application-specific version
python --version  # get current Python version
#pyenv commands  # list of pyenv commands

# Create virtual environment: pyenv virtualenv <python_version> <env_name>
pyenv virtualenv 3.9.16 myproject
pyenv local myproject  # activate environment locally
pyenv versions  # view all environments
#pyenv virtualenv-delete myproject  # delete virtual environment

# Deactivation doesn't work without default system Python.
# Just create another virtual environment and activate it.
pyenv local system  # set system Python locally
#pyenv global system  # set system Python globally
```

#### <a name="tensorflow-pyenv" />Install TensorFlow for GPU using PIP installer

NOTE: Try to not mix PIP and Conda installations together!

For now there is [no](https://stackoverflow.com/a/67912911/7550928)
CUDA Toolkit (`cudatoolkit`) for PIP,
but it is available in the Conda repository.
So we have to mix PIP and Conda installators together.
Install Miniconda and then
[install TensorFlow for GPU using Conda installer](#tensorflow-conda) or
continue and install TensorFlow for GPU with PIP like in
[official tutorial](https://www.tensorflow.org/install/pip).

```shell
pyenv install -v miniconda3-latest  # install Miniconda
pyenv virtualenv miniconda3-latest tf  # create virtual env
pyenv local tf  # activate virtual environment
pyenv versions  # view all environments

# Initialize Miniconda in the ~/.bashrc file
~/.pyenv/versions/miniconda3-latest/bin/conda init bash
exec $SHELL  # restart the shell
conda --version  # check it
#conda update -n base -c defaults conda  # update Conda if necessary

# Update the pip package manager
pyenv which pip  # check pip location
pip install --upgrade pip

# There is no CUDA Toolkit for PIP. Install it with Conda
conda search -c conda-forge cudatoolkit
conda install -c conda-forge cudatoolkit=11.8

# Install cuDNN and TensorFlow with GPU support
pip install nvidia-cudnn-cu11 tensorflow

# Configure the system paths to activate when running the virtual environment
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> \
    $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib' >> \
    $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh

# Verify the GPU setup
python -c "import tensorflow as tf; print('\n' + str(len(tf.config.list_physical_devices('GPU'))) + ' GPU available\n')"

# Install other Python packages
pip install tensorflow-hub matplotlib scipy numpy opencv-python pillow \
    scikit-learn scikit-image pandas ipython jupyter tqdm graphviz

```

</details>


---
### <a name="anaconda" />(OLD) Anaconda virtual environment

<details>
  <summary>Show details</summary>

For TensorFlow try to use Miniconda, because it is smaller.

Links:
   - [How to Install Anaconda on Ubuntu 20.04](https://tecnstuff.net/how-to-install-anaconda-on-ubuntu-20-04/)
   - [How To Install Anaconda on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-anaconda-on-ubuntu-18-04-quickstart)
   - [Anaconda distribution](https://www.anaconda.com/distribution)
   - [Anaconda repository](https://repo.anaconda.com/archive)

#### <a name="install-anaconda" />Install Anaconda

```shell script
# Download latest Anaconda distribution
mkdir -p ~/Downloads/
cd ~/Downloads/
curl -O https://repo.anaconda.com/archive/Anaconda3-2023.03-1-Linux-x86_64.sh

# Run installation script
bash Anaconda3-2023.03-1-Linux-x86_64.sh

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

#### <a name="configure-anaconda" />Configure Anaconda virtual environment

```shell script
# To activate installed Anaconda use command:
source ~/.bashrc

# Set up Anaconda virtual environment
conda create --name myenv python=3.9
# Show virtual envs
conda info --envs
# Activate the environment
conda activate myenv

# Activate and deactivate virtual environment
conda deactivate  # exit to the "base" environment
conda deactivate  # exit from Anaconda base env
conda activate myenv

# Delete vitrual environment if necessary
conda deactivate
conda remove --name myenv --all
conda info --envs
```

</details>

---
### <a name="venv" />(OLD) Common virtual environment

<details>
  <summary>Show details</summary>

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

#### <a name="configure-venv" />Configure virtual environment

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

# Install PyTorch if necessary
# NOTE: check your installation here: https://pytorch.org/
#pip install torch torchvision

# Deactivate myenv
deactivate

# Delete virtual environment if you don't need it any more.
rmvirtualenv myenv
```

</details>

---
### <a name="easy-build" />(OLD) EasyBuild environment on SURFsara server

<details>
  <summary>Show details</summary>

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
conda create --name test python=3.9
# Show virtual environments
conda info --envs
# Activate 'test' environment
source activate test

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

# Deactivate 'test' environment
deactivate

# Delete 'test' virtual environment if you don't need it any more.
rmvirtualenv test
```

To transfer files via `gsiscp` and `gsisftp` read
[this instruction](old/14_Install_GSI_openssh_client.md/#manage).

</details>
