# 深度学习环境内网离线部署

## 安装操作系统
* 下载[Ubuntu 18.04](http://releases.ubuntu.com/18.04/)
* 安装Ubuntu 18.04

## 安装ssh
* 下载ssh
```shell
apt-get download openssh-client
apt-get download openssh-server
apt-get download openssh-sftp-server
```

* 安装ssh
```shell
dpkg -i *.deb
```

## 安装gcc
* 查看gcc的依赖
```shell
apt-get install -s gcc
```

* 下载gcc的依赖 **The following additional packages will be installed:**
```shell
apt-get download binutils binutils-common binutils-x86-64-linux-gnu cpp cpp-7 gcc-7 gcc-7-base libasan4 libatomic1 libbinutils libc-dev-bin libc6-dev libcc1-0 libcilkrts5 libgcc-7-dev libgomp1 libisl19 libitm1 liblsan0 libmpc3 libmpfr6 libmpx2 libquadmath0 libtsan0 libubsan0 linux-libc-dev manpages manpages-dev
```

* 下载gcc
```shell
apt-get download  gcc
```

* 安装gcc
```shell
dpkg -i *.deb
```

## 安装make
* 下载make
```shell
apt-get download  make
```

* 安装make
```shell
dpkg -i *.deb
```

## 安装Nvidia GPU驱动
* 禁⽤nouveau驱动
```shell
编辑配置文件：nano /etc/modprobe.d/blacklist-nouveau.conf 
增加下⾯面内容 
blacklist nouveau options nouveau modeset=0 
sudo update-initramfs -u 
```

* 重启计算机
```shell
reboot
```

* 验证成功
```shell
查看没有任何信息，代表禁⽤用成功
lsmod | grep nouveau 
```

* 下载[Nvidia GPU驱动](https://www.nvidia.com/Download/index.aspx?lang=en-us)

* 安装Nvidia GPU驱动
```shell
sh NVIDIA-Linux-x86_64-418.56.run
```

## 安装Docker-CE
* 下载安装包
```shell
wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/containerd.io_1.2.2-3_amd64.deb
wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce-cli_18.09.2~3-0~ubuntu-bionic_amd64.deb
wget https://download.docker.com/linux/ubuntu/dists/bionic/pool/stable/amd64/docker-ce_18.09.2~3-0~ubuntu-bionic_amd64.deb
```

* 安装Docker-CE
```shell
dpkg -i *.deb
```

## 安装nvidia-docker2
* 下载nvidia-docker2
```shell
apt-get download nvidia-container-runtime
apt-get download nvidia-docker2
```

* 安装nvidia-docker2
```shell
dpkg -i *.deb
```

* 重新启动Docker daemon
```shell
systemctl restart docker
```

## 验证容器内使用GPU
* 镜像保存为tar包
```shell
docker save -o cuda9.0-base.tar nvidia/cuda:9.0-base
```

* 导入镜像到内网计算机
```shell
docker load -i cuda9.0-base.tar
```

* 在容器中查看GPU信息
```shell
docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
```
