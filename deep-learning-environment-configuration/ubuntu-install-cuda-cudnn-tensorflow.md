* 安装[Ubuntu 18.04.1 LTS](http://releases.ubuntu.com/18.04.1/)

* 准备工作
```bash
sudo apt update
sudo apt install nvidia-smi
```

* 禁用nouveau驱动
```bash
sudo nano /etc/modprobe.d/blacklist-nouveau.conf
```
增加下面内容
```txt
blacklist nouveau
options nouveau modeset=0
```
```bash
sudo update-initramfs -u
```

* 重启
查看没有任何信息，代表禁用成功
```bash
lsmod | grep nouveau
```

* 下载[CUDA Toolkit](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal)
* 下载[cuDNN](https://developer.nvidia.com/rdp/cudnn-download)

* 安装CUDA Toolkit
```bash
sudo sh cuda_10.0.130_410.48_linux.run
```

* 配置环境变量
```bash
sudo nano ~/.bashrc
```
在后面增加下面内容
```txt
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:/usr/local/cuda/extras/CPUTI/lib64
export CUDA_HOME=/usr/local/cuda-10.0/bin
export PATH=$PATH:$LD_LIBRARY_PATH:$CUDA_HOME
```
```bash
source ~/.bashrc
```

* 验证CUDA Toolkit安装成功
```bash
cd /usr/local/cuda-10.0/samples/1_Utilities/deviceQuery/
sudo make
./deviceQuery
```

* 安装cuDNN
```bash
sudo dpkg -i libcudnn7_7.4.2.24-1+cuda10.0_amd64.deb 
sudo dpkg -i libcudnn7-dev_7.4.2.24-1+cuda10.0_amd64.deb
sudo dpkg -i libcudnn7-doc_7.4.2.24-1+cuda10.0_amd64.deb
```

* 验证cuDNN安装成功
```bash
cd /usr/src/cudnn_samples_v7/mnistCUDNN/
sudo make
./mnistCUDNN
```

* 安装pip
```bash
# python 2.7
sudo apt install python-pip
# python 3
sudo apt install python3-pip
```

* 安装TensorFlow GPU
```bash
sudo pip3 install tensorflow-gpu
```
