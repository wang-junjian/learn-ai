# 深度学习环境配置
> 操作系统：CentOS 7 显卡：GTX 1060

## 安装 CentOS 7
* 下载 [CentOS](https://www.centos.org/download/)
* 制作U盘启动盘
    运行 UltraISO，[文件]->[打开]，选择CentOS映像打开；[启动光盘]->[写入硬盘映像]，弹出对话模型后，选择U盘，单击写入。
* 安装系统
安装失败显示错误信息：Warning: /dev/root does not exist 解决方法：
1. 查看U盘启动盘的文件名（ls /dev），如果不知道的话，可以运行一次命令，然后拔下U盘，再运行一次命令，对比两次的结果，看看少了哪个文件名。我的U盘文件名是sdb4。
2. 重启系统，回到CentOS安装菜单，在菜单项 Install CentOS 7 按 e 键进行编辑模式，修改linuxefi /images/vmlinuz inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 quiet为linuxefi /images/vmlinuz inst.stage2=hd:/dev/sdb4 quiet
3. 按Ctrl+x，重启系统。

## 安装 NVIDIA 显卡驱动和 CUDA
* 禁用 nouveau
``` shell
sudo nano /usr/lib/modprobe.d/dist-blacklist.conf
blacklist nouveau
options nouveau modeset=0

sudo mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -img)-r.nouveau
sudo dracut /boot/initramfs-$(uname -r).img $(uname -r)
```

* 下载 [CUDA Toolkit](https://developer.nvidia.com/cuda-release-candidate-download)
* 安装 CUDA Toolkit
``` shell
init 3
sudo sh cuda_9.2.88_396.26_linux.run
sudo sh cuda_9.2.88.1_linux.run

sudo nano /etc/profile
export PATH=/usr/local/cuda-9.2/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-9.2/lib64:$LD_LIBRARY_PATH

source /etc/profile
```

* 验证NVIDIA驱动安装成功
``` shell
nvidia-smi
nvidia-settings
```

## 安装 cuDNN
* [下载](https://developer.nvidia.com/rdp/cudnn-download)
* 安装
``` shell
tar -zxvf cudnn-9.2-linux-x64-v7.1.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

## 安装 Python3
* 安装
``` shell
yum search python3
sudo yum install python36
```
* 查看连接
``` shell
ls -li /usr/bin/python36
2487064 lrwxrwxrwx. 1 root root 18 6月   5 22:50 /usr/bin/python36 -> /usr/bin/python3.6
```

* 创建符号连接
``` shell
ln -s python3.6 python3
```

## 安装 pip3
``` shell
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

## 安装 TensorFlow GPU
``` shell
pip3 install tensorflow-gpu
```

## 参考资料
* [U盘安装centos7解决could not boot，/dev/root/does not exit](https://blog.csdn.net/u012140170/article/details/40423645)
* [u盘安装centos7 /dev/root does not exist 导致无法安装解决方案](https://blog.csdn.net/bajiudongfeng/article/details/47732377)
