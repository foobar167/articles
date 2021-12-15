:exclamation: **NOTE**: for Ubuntu 20.04 and 18.04 there is no
need to build TensorFlow from sources.

   - [Task](#task)
   - [Related links](#links)
   - [Previous instructions](#previous)
   - [Install cuDNN](#cuDNN)
   - [Install NCCL](#nccl)
   - [Insstall TensorFlow](#tensorflow)

---
### <a name="task" />Task
   - Install TensorFlow for all users on Ubuntu 18.04.

---
### <a name="links" />Related links

**cuDNN**
   - [cuDNN Installation Guide](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html)
   - [Which NVIDIA cuDNN release type for TensorFlow on Ubuntu 16.04](https://stackoverflow.com/a/48788965/7550928)
   - [Install CUDA 9.2 and cuDNN 7.1 for PyTorch (GPU) on Ubuntu 16.04](https://medium.com/@zhanwenchen/install-cuda-9-2-and-cudnn-7-1-for-tensorflow-pytorch-gpu-on-ubuntu-16-04-1822ab4b2421)

**TensorFlow:**
   - [Install TensorFlow with pip](https://www.tensorflow.org/install/pip)
   - [Part1: How to install Tensorflow GPU with CUDA 10.0 for python on Ubuntu](https://www.python36.com/how-to-install-tensorflow-gpu-with-cuda-10-0-for-python-on-ubuntu)
   - [Part2: How to install Tensorflow GPU with CUDA 10.0 for python on Ubuntu](https://www.python36.com/how-to-install-tensorflow-gpu-with-cuda-10-0-for-python-on-ubuntu/2)

---
### <a name="previous" />Previous instructions
   1. [Prepare Python](../04_Python_installation.md)
   2. [Install software for virtual environments](../05_Virtual_environments.md)
   3. [Prepare GPU driver and CUDA](../08_Nvidia_driver_and_CUDA_install.md)

#### Install Dependencies

```shell
sudo apt update
sudo apt upgrade
sudo apt install python-pip   python3-pip \
                 python2.7    python3 \
                 python-numpy python3-numpy \
                 python-six   python3-six \
                 python-wheel python3-wheel \
                 python-mock  python3-mock 

# Keras — deep learning framework running on Theano or TensorFlow
sudo apt install python3-keras
sudo apt install keras-doc  # documents

# Keras for Python 2.7
sudo su
cd ~
umask 022
sudo pip install -U Keras  # Deep Learning for humans
exit

# Lasagne — deep learning library build on the top of Theano
sudo apt install python-lasagne
sudo apt install python3-lasagne
sudo apt install lasagne-doc  # documents

# Check for necessary packages
python3 --version  # 3.6 or higher
pip3 --version  # 9.0 or higher
virtualenv --version  # 15.1 or higher
nvidia-smi  # NVIDIA driver version
nvcc --version  # CUDA version, 10.0 or higher
which nvcc  # usually /usr/local/cuda/bin/nvcc

# Verify Theano, Keras
python  -c "import theano; print(theano.__version__);"
python3 -c "import theano; print(theano.__version__);"
python  -c "import keras;  print(keras.__version__);"
python3 -c "import keras;  print(keras.__version__);"
```

#### Install Keras for Python 2.7

```shell
# Set permissions 644 for files and 755 for directories
sudo su
cd ~
umask 022
sudo pip install -U --user keras_applications==1.0.5 --no-deps
sudo pip install -U --user keras_preprocessing==1.0.3 --no-deps
```

---
### <a name="cuDNN" />Install cuDNN

Download cuDNN from [nVidia web-site](https://developer.nvidia.com/cudnn)

To download it you have to register, fill the web form and pass the survey.

Choose 3 packages:
   * cuDNN Runtime Library for Ubuntu18.04 (Deb)
   * cuDNN Developer Library for Ubuntu18.04 (Deb)
   * cuDNN Code Samples and User Guide for Ubuntu18.04 (Deb)

Install only runtime library if you using precompiled binaries
that are ready to go. If you want to build, re-build TensonFlow or
develop your own API, install developer library after installation
of runtime library. Install code samples library if you need examples.

```shell
# Install the Runtime library
sudo dpkg -i libcudnn7_7.4.1.5-1+cuda10.0_amd64.deb
# Install the Developer library
sudo dpkg -i libcudnn7-dev_7.4.1.5-1+cuda10.0_amd64.deb
# Install the code samples and the cuDNN Library User Guide
sudo dpkg -i libcudnn7-doc_7.4.1.5-1+cuda10.0_amd64.deb
```

#### Check installation of cuDNN

```shell
# Indicates that CuDNN version 7.4.1 is installed.
cat /usr/include/x86_64-linux-gnu/cudnn_v*.h | grep CUDNN_MAJOR -A 2
#define CUDNN_MAJOR      7
#define CUDNN_MINOR      4
#define CUDNN_PATCHLEVEL 1
--
#define CUDNN_VERSION    (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

#include "driver_types.h"


# Copy the cuDNN sample to a writable path
cp -r /usr/src/cudnn_samples_v7/ $HOME/Documents
# Go to your writable samples
cd  $HOME/Documents/Samples/cudnn_samples_v7/mnistCUDNN
# Compile the mnistCUDNN sample
make clean && make
# Run the mnistCUDNN sample
./mnistCUDNN
```

If cuDNN is properly installed and running on your Linux system,
you will see a message similar to the following:

```shell
Result of classification: 1 3 5
Test passed!
```

#### Add CUPTI to environment variables

```shell
# Create and modify cuDNN.conf file in directory /etc/ld.so.conf.d/
sudo touch /etc/ld.so.conf.d/cuDNN.conf
sudo nano /etc/ld.so.conf.d/cuDNN.conf
```

Add path for CUPTI (**CU**DA **P**rofiling **T**ools **I**nterface)
libraries into cuDNN.conf

```shell
# CUPTI libraries
/usr/local/cuda/extras/CUPTI/lib64
```

Press `<Ctrl>+<X>` and save during exit.

```shell
# Reload ldconfig environment
sudo ldconfig
```

---
### <a name="nccl" />Install NCCL

NVIDIA Collective Communications Library (NCCL) implements multi-GPU
and multi-node collective communication primitives that are
performance optimized for NVIDIA GPUs.

Go to https://developer.nvidia.com/nccl/nccl-download
and attend survey to download Nvidia NCCL.

Download following after completing survey.

*Download NCCL v2.3.7, for CUDA 10.0, Nov 8, 2018* -->
*Local installer for Ubuntu 18.04*.
File: nccl-repo-ubuntu1804-2.3.7-ga-cuda10.0_1-1_amd64.deb

```shell
# Install NCCL deb package
sudo apt install ./nccl-repo-ubuntu1804-2.3.7-ga-cuda10.0_1-1_amd64.deb

# Error
The public CUDA GPG key does not appear to be installed.
To install the key, run this command:
sudo apt-key add /var/nccl-repo-2.3.7-ga-cuda10.0/7fa2af80.pub

# Run command to install the key
sudo apt-key add /var/nccl-repo-2.3.7-ga-cuda10.0/7fa2af80.pub
OK

sudo apt update
sudo apt install libnccl2 libnccl-dev
```

---
### <a name="tensorflow" />Install TensorFlow

There must be 64-bit Python installed.
TensorFlow does not work on 32-bit Python installation.

#### Install through `pip` for CUDA 9.0

With Ubuntu 18.04, using the command `sudo pip install packagename`
does not install into global path. In order to install the modules
in the global path (it keeps looking at the local-user python path):

Unfortunately this installation doesn't work with the latest version
of CUDA, 10.0 at this moment.

```shell
# Change to root user
sudo su
# Change directory to HOME
cd ~
# Set permissions 644 for files and 755 for directories
umask 022
# Install TensorFlow current release with GPU support
#     in the global path for Python 2.7
pip install tensorflow-gpu 
# For Python 3.x
pip3 install tensorflow-gpu
```

#### Install from source for CUDA 10.0

##### Install Bazel

Bazel is an open-source build and test tool similar to Make,
Maven, and Gradle. It uses a human-readable, high-level build
language. Bazel supports projects in multiple languages and builds
outputs for multiple platforms. Bazel supports large codebases
across multiple repositories, and large numbers of users.

[How-to install Bazel for Ubuntu](https://docs.bazel.build/versions/master/install-ubuntu.html#install-with-installer-ubuntu)

Download Bazel Linux installer named
`bazel-<version>-installer-linux-x86_64.sh`
from the Bazel releases page on GitHub:
https://github.com/bazelbuild/bazel/releases

[Downgrade to bazel 0.17.2](https://github.com/tensorflow/tensorflow/issues/24385)
Otherwise it'll be and errors during TensorFlow build.

```shell
# Install required packages
sudo apt install pkg-config zip g++ zlib1g-dev unzip python

# Uninstall Bazel
rm -fr ~/.bazel
rm -fr ~/.cache/bazel
rm ~/bin/bazel

# Install Bazel

# Create ~/Documents/Install/bazel folder
mkdir -p ~/Documents/Install/bazel
cd       ~/Documents/Install/bazel
# Get installer for version 0.17.2
wget https://github.com/bazelbuild/bazel/releases/download/0.17.2/bazel-0.17.2-installer-linux-x86_64.sh
# Set executable
chmod +x bazel-0.17.2-installer-linux-x86_64.sh
# Install for current user. Use --help for additional options
./bazel-0.17.2-installer-linux-x86_64.sh --user
# Set up your environment
echo 'export PATH="$PATH:$HOME/bin"' >> ~/.bashrc

# Reload environment variables
source ~/.bashrc
sudo ldconfig
```

<details close>
  <summary><b>SYCL / CUDA / ROCm are mututally exclusive</b></summary>
    <blockquote>

:exclamation: **SYCL / CUDA / ROCm are mututally exclusive.
At most 1 GPU platform can be configured. Don't install them** :exclamation:

##### Install ComputeCpp if you're going to build TensorFlow with OpenCL support

In other words you could create either CUDA or ComputeCPP,
but not in the same time.

Install OpenCL and OpenGL for Python

```shell
# OpenCL and OpenGL for Python

# OpenCL (Open Computing Language) header files
sudo apt install opencl-headers
# OpenCL (Open Computing Language) C header files
sudo apt install opencl-c-headers
# C++ headers for OpenCL development
sudo apt install opencl-clhpp-headers
# Python module to access OpenCL parallel computation API
sudo apt install python-pyopencl
# Python 3 module to access OpenCL parallel computation API
sudo apt install python3-pyopencl
```

Install ComputeCpp.
Download [ComputeCpp Community Edition](https://www.codeplay.com/products/computesuite/computecpp)
from CodePlay website.

```shell
# Get ComputeCpp-CE-1.0.3-Ubuntu.16.04-64bit.tar.gz and
tar xvzf ComputeCpp-CE-1.0.3-Ubuntu.16.04-64bit.tar.gz
cd ComputeCpp-CE-1.0.3-Ubuntu-16.04-x86_64/

# Copy files and dirs to /usr/local/
sudo cp -a ./bin/. /usr/local/bin/
sudo cp -a ./doc/. /usr/local/doc/
sudo cp -a ./include/. /usr/local/include/
sudo cp -a ./lib/. /usr/local/lib/

# Check installation
computecpp_info
```
---
  </blockquote>
</details>
<br/>

##### Install TensorRT if you're going to build TensorFlow with TensorRT support

Install cuBLAS — Dense Linear Algebra on GPUs
Download CUDA Toolkit with cuBLAS: https://developer.nvidia.com/cublas

```shell
sudo apt install ./cuda-repo-ubuntu1804-10-0-local-10.0.130-410.48_1.0-1_amd64.deb
sudo apt update
sudo apt install cuda

sudo reboot
```

Download TensorRT file from NVIDIA website https://developer.nvidia.com/tensorrt

```shell
# Install TensorRT
sudo apt install ./nv-tensorrt-repo-ubuntu1804-cuda10.0-trt5.0.2.6-ga-20181009_1-1_amd64.deb

sudo apt update
#sudo apt upgrade

sudo apt install tensorrt

sudo apt install \
    libnvinfer5 \
    libnvinfer-dev \
    python-libnvinfer \
    python3-libnvinfer \
    python-libnvinfer-dev \
    python3-libnvinfer-dev \
    uff-converter-tf

# Verify installation
dpkg -l | grep -i TensorRT
```

##### Download and configure TensorFlow

```shell
# Install TensorFlow

# Create ~/Documents/Install/TensorFlow folder
mkdir -p ~/Documents/Install/TensorFlow
cd       ~/Documents/Install/TensorFlow
# Get sources
git clone https://github.com/tensorflow/tensorflow.git
cd tensorflow
# Switch to stable version of TensorFlow, current is r1.12
# TensorFlow API Versions: https://www.tensorflow.org/versions
git checkout r1.12

./configure
```

Answer for the following questions:

```text
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3
Please input the desired Python library path to use.  Default is [/usr/lib/python3/dist-packages]
/usr/lib/python3/dist-packages
Do you wish to build TensorFlow with Apache Ignite support? [Y/n]: Y
Do you wish to build TensorFlow with XLA JIT support? [Y/n]: Y

# Warning: CUDA and SYCL are mututally exclusive.
Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: N
Do you wish to build TensorFlow with ROCm support? [y/N]: N
Do you wish to build TensorFlow with CUDA support? [y/N]: Y
Please specify the CUDA SDK version you want to use. [Leave empty to default to CUDA 9.0]: 10.0
Please specify the location where CUDA 10.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda
Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7]: 7.4.1
Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda

Do you wish to build TensorFlow with TensorRT support? [y/N]: Y
  Please specify the location where TensorRT is installed. [Default is /usr/lib/x86_64-linux-gnu]: /usr/lib/x86_64-linux-gnu
  Please specify the NCCL version you want to use. If NCCL 2.2 is not installed, then you can use version 1.3 that can be fetched automatically but it may have worse performance with multiple GPUs. [Default is 2.2]: 2.3.7

NCCL libraries found in /usr/lib/x86_64-linux-gnu/libnccl.so
This looks like a system path.
Assuming NCCL header path is /usr/include
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 6.1]: 5.2,6.1

Do you want to use clang as CUDA compiler? [y/N]: N
Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/x86_64-linux-gnu-gcc-6]: /usr/bin/gcc

Do you wish to build TensorFlow with MPI support? [y/N]: N

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: -march=native
Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: N
```

<details close>
  <summary>Full input and output text</summary>
    <blockquote>

```text
./configure
WARNING: An illegal reflective access operation has occurred
WARNING: Illegal reflective access by com.google.protobuf.UnsafeUtil (file:/home/lab225/.cache/bazel/_bazel_lab225/install/792a28b07894763eaa2bd870f8776b23/_embedded_binaries/A-server.jar) to field java.lang.String.value
WARNING: Please consider reporting this to the maintainers of com.google.protobuf.UnsafeUtil
WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
WARNING: All illegal access operations will be denied in a future release
WARNING: --batch mode is deprecated. Please instead explicitly shut down your Bazel server using the command "bazel shutdown".
You have bazel 0.17.2 installed.
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3


Found possible Python library paths:
  /usr/lib/python3.6/dist-packages
  /usr/lib/python3/dist-packages
  /usr/local/lib/python3.6/dist-packages
Please input the desired Python library path to use.  Default is [/usr/lib/python3.6/dist-packages]
/usr/lib/python3/dist-packages
Do you wish to build TensorFlow with Apache Ignite support? [Y/n]: Y
Apache Ignite support will be enabled for TensorFlow.

Do you wish to build TensorFlow with XLA JIT support? [Y/n]: Y
XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: N
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with ROCm support? [y/N]: N
No ROCm support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: Y
CUDA support will be enabled for TensorFlow.

Please specify the CUDA SDK version you want to use. [Leave empty to default to CUDA 9.0]: 10.0


Please specify the location where CUDA 10.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda


Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7]: 7.4.1


Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: /usr/local/cuda


Do you wish to build TensorFlow with TensorRT support? [y/N]: Y
TensorRT support will be enabled for TensorFlow.

Please specify the location where TensorRT is installed. [Default is /usr/lib/x86_64-linux-gnu]:/usr/lib/x86_64-linux-gnu


Please specify the NCCL version you want to use. If NCCL 2.2 is not installed, then you can use version 1.3 that can be fetched automatically but it may have worse performance with multiple GPUs. [Default is 2.2]: 2.3.7


NCCL libraries found in /usr/lib/x86_64-linux-gnu/libnccl.so
This looks like a system path.
Assuming NCCL header path is /usr/include
Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 3.5,7.0]: 5.2,6.1


Do you want to use clang as CUDA compiler? [y/N]: N
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/x86_64-linux-gnu-gcc-6]: /usr/bin/gcc


Do you wish to build TensorFlow with MPI support? [y/N]: N
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: -march=native


Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: N
Not configuring the WORKSPACE for Android builds.

Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See tools/bazel.rc for more details.
	--config=mkl         	# Build with MKL support.
	--config=monolithic  	# Config for mostly static monolithic build.
	--config=gdr         	# Build with GDR support.
	--config=verbs       	# Build with libverbs support.
	--config=ngraph      	# Build with Intel nGraph support.
Configuration finished
```
---
  </blockquote>
</details>
<br/>

For your information:
   * TensorRT is installed in the same folder as file `libnvinfer.so`.
It is `/usr/lib/x86_64-linux-gnu` by default.

   * Show version of NCCL: `dpkg -l | grep -i nccl` (2.3.7).

   * Show where NCCL library is installed: `find / -name '*libnccl*' 2>/dev/null`
(/usr/lib/x86_64-linux-gnu).

   * Compute capability for `Nvidia GeForce GTX TITAN X` is `5.2`.
   * Compute capability for `Nvidia GeForce GTX 1080 Ti` is `6.1`
(see https://developer.nvidia.com/cuda-gpus).

   * I have not used MPI support myself.
Read ```tensorflow/contrib/mpi/README.md``` file before MPI installation.

##### Build Tensorflow using bazel

Build a pip package for TensorFlow

```shell
cd ~/Documents/Install/TensorFlow/tensorflow

# Create $TMP variable for Bazel version 0.17.2
export TMP=~/tmp
mkdir -p $TMP

bazel build --config=opt  \
            --config=cuda \
            //tensorflow/tools/pip_package:build_pip_package
```

If you have the WARNING:
```text
WARNING: The following rc files are no longer being read,
please transfer their contents or import their path into one of the standard rc files:
~/Documents/Install/TensorFlow/tensorflow/tools/bazel.rc
```

[This is an issue with bazel 0.19.0](https://github.com/tensorflow/tensorflow/issues/23401),
either use bazel 0.18.0, or
add the contents of file `~/Documents/Install/TensorFlow/tensorflow/tools/bazel.rc`
on top of (hidden) file  `~/Documents/Install/TensorFlow/tensorflow/.tf_configure.bazelrc`.

I used Bazel version 0.17.2. It's old, but it works for now (date 2019.02.13).

```shell
# Create backup
cd ~/Documents/Install/TensorFlow/tensorflow
cp .bazelrc .bazelrc.2019.02.13.backup

# Import path to bazel.rc file on top of hidden .bazelrc
echo import ~/Documents/Install/TensorFlow/tensorflow/tools/bazel.rc > temp_file.txt
cat .bazelrc >> temp_file.txt
mv temp_file.txt .bazelrc

# Build a pip package for TensorFlow
bazel build --config=opt  \
            --config=cuda \
            //tensorflow/tools/pip_package:build_pip_package
```

Building time for CPU Intel Core i7-7800X: 1 hour and 10 min

Build whl (wheel) file

```shell
bazel-bin/tensorflow/tools/pip_package/build_pip_package tensorflow_pkg_python3
```

Install tensorflow with `pip`

```shell
# Install TensorFlow for all users
cd ~/Documents/Install/TensorFlow/tensorflow
sudo mkdir -p /root/Install/tensorflow_pkg_python3
sudo cp -a tensorflow_pkg_python3/. /root/Install/tensorflow_pkg_python3
sudo su
cd ~
umask 022
cd ~/Install/tensorflow_pkg_python3
sudo pip3 install tensorflow*.whl
exit

# For virtual environment use
##sudo apt-get install virtualenv
##virtualenv tf_1.12_cuda10.0 -p /usr/bin/python3
##source tf_1.12_cuda10.0/bin/activate
##pip3 install tensorflow*.whl
```

##### Build Tensorflow for Python 2.7

There are several differences:

```text
# During config enter path to Python 2.7
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python
Please input the desired Python library path to use.  Default is [/usr/lib/python2.7/dist-packages]
/usr/lib/python2.7/dist-packages
```

```shell
# Build whl (wheel) file
bazel-bin/tensorflow/tools/pip_package/build_pip_package tensorflow_pkg_python2

# Install TensorFlow for Python 2.7 and for all users
cd ~/Documents/Install/TensorFlow/tensorflow
sudo mkdir -p /root/Install/tensorflow_pkg_python2
sudo cp -a tensorflow_pkg_python2/. /root/Install/tensorflow_pkg_python2
sudo su
cd ~
umask 022
cd ~/Install/tensorflow_pkg_python2
sudo pip install tensorflow*.whl
exit
```

#### Make some tests to check installation of TensorFlow

```shell
# Check for Python 3.6
python3.6 -c "import tensorflow as tf;  \
    tf.enable_eager_execution();        \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"

# Check for Python 2.7
python2.7 -c "import tensorflow as tf;  \
    tf.enable_eager_execution();        \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"
```

There could be runtime error, which reports that NumPy is too old:
```text
RuntimeError: module compiled against API version 0xc but this version of numpy is 0xb
RuntimeError: module compiled against API version 0xc but this version of numpy is 0xb
```

In this case you should reinstall NumPy through `pip`
and **not** through `sudo apt install python-numpy`.

In my case I had NumPy 1.13 installed through `apt`.
However NumPy 1.15 is required for TensorFlow 1.12.
So I have to reinstall NumPy through `pip` for Python 2.7.

```shell
# Uninstall python-numpy (NumPy 1.13 for Python 2.7)
sudo apt purge --auto-remove python-numpy

# There is an error when "import tensorflow as tf" for Python 2.7
# Error: Couldn't import dot_parser, loading of dot files will not be possible.
# To fix it, uninstall pydot2 and install pydot instead
# But first uninstall pydot through APT
sudo apt purge --auto-remove python-pydot

# Reinstall all necessary packages through pip
sudo su
cd ~
umask 022

# Reinstall packages via pip for Python 2.7
sudo pip install -U numpy  # NumPy 1.15
sudo pip install -U scipy
sudo pip install -U matplotlib
sudo pip install -U scikit-image
sudo pip install -U scikit-learn
sudo pip install -U opencv-contrib-python
sudo pip install -U pandas

sudo pip install -U Theano
sudo pip install -U Lasagne
sudo pip install -U TheanoLM

# Hierarchical datasets for Python
sudo pip install -U tables
# A collection of tools for Python
sudo pip install -U pytools
# Statistical computations and models for Python
sudo pip install -U statsmodels
# Python wrapper for OpenCL — Compilation failure
##sudo pip install -U pyopencl
# Python Webtrends connector — No matching version
##sudo pip install -U pywt

# Fix error with import of dot_parser
sudo pip uninstall pydot2
sudo pip install pydot

exit  # exit from root

# Check installation of TensorFlow for Python 2.7 once again
python2.7 -c "import tensorflow as tf;  \
    tf.enable_eager_execution();        \
    print(tf.reduce_sum(tf.random_normal([1000, 1000])));"
```
