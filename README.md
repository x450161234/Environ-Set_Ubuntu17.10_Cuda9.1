# Environ-Set_Ubuntu17.10_Cuda9.1
Environment setup for Lenovo R720(Nvidia 1050ti) with CUDA9.1

## Nvidia Driver
### Prerequisit Fot Nvidia Driver
"""
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libopenblas-dev liblapack-dev libatlas-base-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt-get install git cmake build-essential
"""

### Block nouveau
open file
"""
sudo gedit /etc/modprobe.d/blacklist-nouveau.conf
"""
add content into the file
"""
blacklist amd76x_edac 

blacklist vga16fb
blacklist nouveau
options nouveau modeset=0
blacklist rivafb
blacklist nvidiafb
blacklist rivatv
"""
set them work
"""
sudo update-initramfs -u
"""

set the path
"""
sudo gedit ~/.bashrc
"""
add
"""
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH
"""
run
"""
source ~/.bashrc
"""

turn off the Graphical Desktop
"""
sudo /etc/init.d/gdm3 stop
sudo init 3
"""
install Nvidia Graphical Driver(.run)
"""
查看自己的显卡型号：
sudo lshw -numeric -C display
如果之前安装过nvidia驱动，先卸载：
sudo apt-get remove –purge nvidia*
Ctrl-Alt+F1进入命令行界面
cd Downloads
sudo chmod a+x /home/daniel/Downloads/NVIDIA-Linux-x86_64-390.25.run
sudo .//home/daniel/Downloads/NVIDIA-Linux-x86_64-390.25.run –no-x-check –no-nouveau-check –no-opengl-files
# 这句一定要加参数，不然就会循环登录
"""
## CUDA installation
### prerequist & installation
"""
sudo apt-get install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev
sudo apt install gcc-6
sudo apt install g++-6
sudo chmod a+x /home/daniel/Downloads/cuda_9.1.85_387.26_linux.run
sudo ./home/daniel/Downloads/cuda_9.1.85_387.26_linux.run --override
"""
path setup
"""
sudo gedit ~/.bashrc
add：
export PATH=/usr/local/cuda-8.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
run： source ~/.bashrc
"""
安装CUDA完毕后，执行如下命令：
"""
sudo cp -rf /etc/ld.so.conf /etc/ld.so.conf.bak
sudo vim /etc/ld.so.conf

添加CUDA 9库文件到LD_LIBRARY_PATH:

include /usr/local/cuda-9.0/lib64

然后因为CUDA 9.0只支持gcc -v6，所以我们得按照如下命令设定：

sudo ln -s /usr/bin/gcc-6 /usr/local/cuda/bin/gcc
sudo ln -s /usr/bin/g++-6 /usr/local/cuda/bin/g++
"""
### install CUDNN v7
"""
tar -zxvf cudnn-9.0-linux-x64-v7.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
"""
configure path
"""
export PATH=${PATH}:/usr/local/cuda-9.0/bin
export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}/usr/local/cuda-9.0/lib64:/usr/local/cuda-9.0/lib:/usr/local/cuda/extras/CUPTI/lib64
"""

### Reference:
[ubuntu 17.10 + CUDA 9.0 + cuDNN 7 + tensorflow源码编译](https://zhuanlan.zhihu.com/p/30781460)
[双显卡 ubuntu16.04 安装 NVIDIA驱动 + CUDA + cuDNN + tensorflow-gpu + keras](https://zhuanlan.zhihu.com/p/30781460)
