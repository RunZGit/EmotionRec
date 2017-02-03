# Cuda, Caffe and OpenCV Setup Tutorial
A lot people are having trouble with intalling all this software, and there is not a complete tutorial online that is simple enough to understand. So, if you wanna skip through the reading and just wants to get them to work, follow this tutorial, and hopfully other people could have less of pain installing those tools. Happy data crunching.
# Install Cuda
In case you love reading, I have linked the two original documents for cuda8.0, for your enlighments. Warning, those links are not kept up to date, google the new ones if they are expired. If you still cannot find it, I reconmend you close your computer, and just give up.    
[NVIDIA CUDA INSTALLATION GUIDE FOR LINUX](http://docs.nvidia.com/cuda/cuda-installation-guide-linux/#axzz4Xe5BN9U9)   
[CUDA QUICK START GUIDE](http://docs.nvidia.com/cuda/cuda-quick-start-guide/#axzz4Xe5BN9U9)   
## Requirements 
Ubuntu 16.04 or 14.04
Cuda compatible graphics card. To check your graphics card compatibility, [click here](https://developer.nvidia.com/cuda-gpus)
This tutorial mainly focus on ubuntu 16.04 version 
## Installation Steps
First, download the newest version of CUDA [here](https://developer.nvidia.com/cuda-downloads). This tutorial is writen when cuda 8.0 is the newest version availble. I downloaded the linux x86_64 ubuntu 16.04 deb local file.
```bash
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local_8.0.44-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda
```
Now we need to setup the environment.
```bash
cd ~
vi .bashrc
# add this line in your .bashrc file
 export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
 export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```
Restart your terminal, or do source reactivate if you are fancy. Now we are going to test if Cuda is working on your computer.
```bash
# simple version check
 cat /proc/driver/nvidia/version
# (optional) compiling examples
# If your cuda samples is installed in a different dirctory, try to search it. If you still cannot find it, just google how to find it.
cd  /usr/local/cuda-8.0/samples/0_Simple/vectorAdd
# I dont know why I can't just use make, but this following does the job
sudo make
./vectorAdd
# Something will print out, if it says success, congratulations!!! If not, go to the official documents, I can't help you.
# For those who has OCDs, and wants to remove the unneeded file after this 
sudo make clean
```
## Install cuDNN
Go to this [link](https://developer.nvidia.com/rdp/cudnn-download) to download the CuDNN. Select the cuDNN v* Library for Linux. I was reading through this [link](http://www.pyimagesearch.com/2016/07/04/how-to-install-cuda-toolkit-and-cudnn-for-deep-learning/) while installing this.
Once downloaded
```bash
tar -zxf cudnn-8.0-linux-x64-v5.0-ga.tgz
cd cuda
sudo cp lib64/* /usr/local/cuda/lib64/
sudo cp include/* /usr/local/cuda/include/
```
Now you are good to remove the installing files.
# Caffe
Note: You need Anaconda with python2.7 version for this. Installing opencv for python3 is a pain, if you want to skip this tutorial because I am using python2.7, think again. I had the same thought when I saw other [tutorial](https://yangcha.github.io/Caffe-Conda/), but in order to install opencv for python3, you need build the package yourself, if you are confident with you C skills, go ahead, otherwise, this is the easiest way to do it. To install Anaconda, follow this [link](https://www.continuum.io/downloads).
The dependcies are listed in this [link](http://caffe.berkeleyvision.org/installation.html), I just kinda followed it
(Optional) If you like a clean environment do the following, it might be a little more work, but it wont affect your original conda environments.
## Requirements
If you think you can install those calling conda install, then 
# NONONONONONONON!!!
Don't you do it, you will regret for the rest of your life, like me sitting in the CS lab 2:50 in the morning, took me 8 hours to figure this out!!! So don't do it!!!!
```bash
#I called it caffe, you can call it whatever you want
conda create -n caffe python=2 anaconda
source activate caffe
sudo apt-get install -y build-essential cmake git pkg-config
sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev protobuf-compiler
sudo apt-get install -y libatlas-base-dev 
sudo apt-get install -y --no-install-recommends libboost-all-dev
sudo apt-get install -y libgflags-dev libgoogle-glog-dev liblmdb-dev

# Also, you need cmake
sudo apt-get install cmake
```
## Downloading Caffe
```
git clone https://github.com/BVLC/caffe.git
cd caffe
mkdir build
cd build
cmake ..
```
This is gonna take forever, I watched people argued with 3 algorithm exam questions and the installation was still not finished.
My build failed in the middle because protobuf was not found, make sure you use the sudo apt-get instead of conda install. Don't ask me why. If you are bored, here is a joke, one of my professor gave us the most depressing definition of computer science majors: all we do is editing text files for a living.
```bash
make all
make install
make runtest
```
If you have trouble with GLIBCXX_3.4.2* not found, enter the following command
```bash
conda install libgcc
```
After that 2000 tests, we are almost done
```bash
cd {path to caffe}/caffe/python
for req in $(cat requirements.txt); do pip install $req; done
pip install lmdb
cd ~
vi .bashrc
#add this line
PYTHONPATH={path to caffe}/caffe/python:$PYTHONPATH
```

