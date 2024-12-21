:exclamation: **Warning**: use [virtual environments](../05_Virtual_environments.md)
instead of installing packages on default Python version.

   - [Task](#task)
   - [`apt` versus `pip` installer](#versus)
   - [Python 3.x installation](#python)
   - [Install additional packages](#packages)
   - [Install and upgrade via `pip`](#pip)
   - [Check installations](#check)
   - [Search and remove packages](#additional)

---
### <a name="task" />Task

Install Python and all needed packages on Ubuntu 18.04 and 20.04.
[My answer](https://askubuntu.com/a/1097135/672237) on AskUbuntu.com.

---
### <a name="versus" />`apt` versus `pip` installer

Cases to install Python packages on default Python version via
`apt` command instead of `pip` command:
   1. `apt` installs only tested on Ubuntu packages and depencences;
   2. `sudo apt` `update` / `upgrage` command keeps packages up to date;
   3. if you want install / update packages for all users on Ubuntu system,
      not only for your own local account;
   4. if you want packages for Ubuntu, so operating system could use them too.

For other Python versions it is better to use
[virtual environment](../05_Virtual_environments.md)
or build and test packages from the source codes
(_for specialists only_).

---
### <a name="python" />Python 3.x installation

Update Ubuntu before every significant software installation.

```shell script
# Refresh repositories
sudo apt update
# Update software
sudo apt upgrade
```

Install Pip and then install Python.

```shell script
# Install pip for 3.x
sudo apt install python3-pip
# Install currently supported by Ubuntu python 3.x version.
sudo apt install python3
```

:exclamation: **Don't delete current python3,
otherwise Ubuntu OS will _BROKE_.** :exclamation:

<details close>
  <summary>Configure the newer versions 3.7, 3.8, 4.0, etc
  only into virtual environment. Don't install packages like
  NumPy globally (on the whole OS) for the newer versions.
  </summary>
    <blockquote>

These commands work for Ubuntu 18.04, but not for 20.04:

```shell script
# Install only minimal versions 3.7, 3.8, etc.
sudo apt install python3.7
sudo apt install python3.7-venv
sudo apt install python3.8
sudo apt install python3.8-venv
python3.7 --version
python3.8 --version
```

It's a bad idea to use several versions globally, because in this case
import of NumPy (`import numpy`) and other modules will fail for
additional Python versions.

For example, if default Python version is 3.6,
then installed via `apt` NumPy package will not work for 3.7.
So it'll fail to import in the 3.7 version when you'll try.

```shell script
# For example:
sudo apt install python3.7  # bad idea - 3.6 and 3.7 at the same time
sudo apt install python3-numpy  # install NumPy for 3.6, not 3.7
python3.7  # call 3.7
import numpy
# Error!
Traceback (most recent call last):
  File "/usr/lib/python3/dist-packages/numpy/core/__init__.py", line 16, in <module>
    from . import multiarray
ImportError: cannot import name 'multiarray' from 'numpy.core'
(/usr/lib/python3/dist-packages/numpy/core/__init__.py)

# Remove python 3.7
sudo apt remove python3.7
sudo apt autoremove
```

If you need 3.7 or newer, install the **minimal versions** and use
[**local virtual environment**](../05_Virtual_environments.md) to install other packages.
It's a bad idea to have several versions of python 3.x globally
at the same time. Use only currently supported
by Ubuntu python 3.x version globally. At this moment it is 3.6
for Ubuntu 18.04 and 3.8 for Ubuntu 20.04.

---
  </blockquote>
</details>
<br/>

---
### <a name="packages" />Install additional packages

Install additional packages. Choose which package you need for work.
Here are most common packages from everyday usage. However you could
have another list.

```shell script
# Install packages:
sudo apt install python-numpy  # numpy
sudo apt install python3-numpy
sudo apt install python-scipy  # scipy
sudo apt install python3-scipy
sudo apt install python-matplotlib  # matplotlib
sudo apt install python3-matplotlib
sudo apt install python-sklearn  # scikit-learn
sudo apt install python3-sklearn
sudo apt install python-skimage  # scikit-image
sudo apt install python3-skimage
sudo apt install python-opencv  # opencv
sudo apt install python3-opencv
sudo apt install python-pandas  # pandas
sudo apt install python3-pandas
sudo apt install python-pil  # pillow
sudo apt install python3-pil
sudo apt install python-pil.imagetk  # if the imageTk import doesn't work
sudo apt install python3-pil.imagetk
sudo apt install python-psutil  # psutil
sudo apt install python3-psutil
sudo apt install python-spur  # spur
sudo apt install python3-spur
sudo apt install cython  # cython
sudo apt install cython3
sudo apt install python-ipython  # ipython
sudo apt install python3-ipython
sudo apt install ipython
sudo apt install ipython3
sudo apt install jupyter  # jupyter
sudo apt install git  # git
sudo apt install python-yaml python3-yaml  # YAML parser and emitter for Python

# Mocking and testing library
sudo apt install python-mock
sudo apt install python3-mock

# pydot is a Python interface to Graphviz's dot
# pydot is needed for TensorFlow
sudo apt install graphviz
sudo apt install python-pydot
sudo apt install python3-pydot
sudo apt install python-pyparsing
sudo apt install python3-pyparsing

# To have both python 2.x and 3.x available on jupyter
sudo apt install python-ipykernel
sudo apt install python3-ipykernel
```

Install it ALL with one command.
However try to use [virtual environments](../05_Virtual_environments.md) instead
to escape possible problems.

```shell script
# Install it ALL with one command :-)
# However try to use virtual environments to escape possible problems.
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
                 python-yaml        python3-yaml
```

Install TensorFlow for all users on the operating system,
but you should have [CUDA and cuDNN installed](../08_Nvidia_driver_and_CUDA_install.md)
beforehand:

```shell script
# Install CUDA beforehand
# Install cuDNN beforehand

# Change to root user
sudo su
# Change directory to HOME
cd ~
# Set permissions 644 for files and 755 for directories
umask 022
# Install TensorFlow current release with GPU support
# in the global path for Python 2.7
pip install tensorflow-gpu 
# For Python 3.x
pip3 install tensorflow-gpu

# Check it
python -c "import tensorflow as tf;     \
    print('Vertion:', tf.__version__);  \
    print(tf.reduce_sum(tf.random.normal([1000, 1000])));"
python3 -c "import tensorflow as tf;    \
    print('Vertion:', tf.__version__);  \
    print(tf.reduce_sum(tf.random.normal([1000, 1000])));"
```

---
### <a name="pip" />Install and upgrade via `pip`

See also [set up and configure Anaconda virtual environment](05_Virtual_environments.md/#configure-anaconda)

Install and upgrade packages via `pip` under Windows OS.

```shell script
# Upgrade pip
python -m pip install --upgrade pip
# Install packages
pip install numpy scipy matplotlib scikit-learn scikit-image opencv-contrib-python pandas pillow psutil spur cython ipython jupyterlab python-git ipyparallel pyyaml virtualenvwrapper-win pipenv tensorflow-gpu

# Check TensorFlow
python -c "import tensorflow as tf; print('Vertion:', tf.__version__); tf.enable_eager_execution(); print(tf.reduce_sum(tf.random_normal([1000, 1000])));"

# Update all pip packages
pip freeze > requirements.txt && pip install --upgrade -r requirements.txt && rm requirements.txt

# Install packages for Anaconda
# Add write permission to C:\ProgramData\Anaconda3
conda install numpy scipy matplotlib scikit-learn scikit-image pandas pillow psutil cython ipython jupyterlab ipyparallel pyyaml tensorflow-gpu

# For OpenCV try different repositories
conda install -c michael_wild opencv-contrib
conda install -c conda-forge opencv
# or
conda activate
pip install opencv-contrib-python
conda deactivate
# or install OpenCV in Anaconda virtual environment
https://github.com/foobar167/articles/blob/master/Ubuntu/05_Virtual_environments.md/#configure-anaconda
```

Upgrade packages for Anaconda under Windows OS.

```shell script
conda update --prefix C:\ProgramData\Anaconda3 anaconda
```

Upgrade packages for Anaconda under Ubuntu OS.

```shell script
# Update conda and then anaconda
conda update
anaconda update
# Update all packages to the last version (this can lead to an unstable environment)
conda update --all
# Update all packages for myenv virtual environment
conda update -n myenv --all
```

---
### <a name="check" />Check installations

```shell script
# Check installation for Python header files and a static library
sudo apt install python-dev
sudo apt install python3-dev
```

```shell script
# Temporary set environment variable
export PATH=/usr/bin:$PATH
# To check python 3.x run
python3

# Then type in python 2.x or 3.x console
import numpy       # check NumPy
import scipy       # check SciPy
import matplotlib  # check MatPlotLib
import sklearn     # check Scikit-learn
import skimage     # check Scikit-image
import cv2         # check OpenCV
import pandas      # check Pandas
from PIL import ImageTk  # check Pillow
import psutil      # check PsUtil
import spur        # check Spur
import cython      # check Cython
exit()
```

```shell script
# To check IPython run
ipython3
exit
```

```shell script
# To check Jupyter run
jupyter notebook
# and check both python versions 2.x and 3.x in "New" menu of the browser.
```

```shell script
# To check GIT run
git --version
```

---
### <a name="additional" />Search and remove packages

```shell script
# To remove package (don't remove python3 — it'll broke your Ubuntu)
#sudo apt purge --auto-remove packagename
sudo apt remove packagename
# To search for the packagename
apt search packagename
```
