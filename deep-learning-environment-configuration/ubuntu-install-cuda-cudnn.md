# 在Ubuntu上安装CUDA和cuDNN

* 安装Ubuntu
[Ubuntu 16.04](http://releases.ubuntu.com/16.04/)
[Ubuntu 18.04](http://releases.ubuntu.com/18.04/)

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

* 下载
[CUDA Toolkit](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal)
[cuDNN](https://developer.nvidia.com/rdp/cudnn-download)

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

* 卸载Nvidia驱动
```bash
nvidia-uninstall
```

* 安装pip
```bash
# python 2.7
sudo apt install python-pip
# python 3
sudo apt install python3-pip
```

* 安装[TensorFlow](https://www.tensorflow.org/install/)
```bash
sudo pip3 install tensorflow-gpu
```

* 安装[PyTorch](https://pytorch.org)
```bash
sudo pip3 install https://download.pytorch.org/whl/cu100/torch-1.0.0-cp36-cp36m-linux_x86_64.whl
sudo pip3 install torchvision
```

* 运行pytorch-cifar
```bash
git clone https://github.com/kuangliu/pytorch-cifar.git
python3 main.py --lr=0.01
```

* 监控GPU
```bash
watch -n 0.1 -d nvidia-smi
```

## 参考资料
* [High Performance Computing](https://developer.nvidia.com/computeworks)
* [Ubuntu18+cuda9.0+cudnn+tensorflow+GPU（1080Ti）+protobuf-3.6.0](https://blog.csdn.net/m0_37407756/article/details/80769952)
* [深度学习环境配置:Ubuntu16.04安装GTX1080Ti+CUDA9.0+cuDNN7](https://www.cnblogs.com/tanwc/p/9375161.html)
* [Ubuntu16.04+1080(Ti)显卡驱动+CUDA+cuDNN](https://blog.csdn.net/lwplwf/article/details/79881699)
* [搭建深度学习服务器Ubuntu 16.04 LTS + GTX 1080Ti](https://www.jianshu.com/p/4e64cb45a5a4)
* [Ubuntu16.04 + 1080Ti深度学习环境配置教程](https://www.jianshu.com/p/5b708817f5d8)
* [ubuntu 16.04 安装 gtx 1080ti](https://blog.csdn.net/lewif/article/details/79083452)
* [Ubuntu 16.04安装NVIDIA驱动后导致的循环登录问题](https://blog.csdn.net/gavinmiaoc/article/details/79748689)
* [nvidia-smi 实时刷新 实时显示显存使用情况](https://blog.csdn.net/sinat_26871259/article/details/82684582)
* [Login failure, kick me back to greeter](https://ubuntuforums.org/showthread.php?t=2361640)