# 在容器中使用NVIDIA GPU

## 安装CUDA和cuDNN
* [在Ubuntu上安装CUDA和cuDNN](ubuntu-install-cuda-cudnn.md)

## 安装 Docker CE
* [Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)

## 安装 nvidia-docker2 (NVIDIA Container Runtime for Docker)
* 安装
```bash
# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
# sudo pkill -SIGHUP dockerd
sudo systemctl restart docker
```

* 测试
```bash
# Test nvidia-smi with the latest official CUDA image
sudo docker run --runtime=nvidia --rm nvidia/cuda:9.0-base nvidia-smi
Wed Jan 23 04:01:09 2019       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 410.48                 Driver Version: 410.48                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 108...  Off  | 00000000:02:00.0 Off |                  N/A |
| 25%   30C    P0    60W / 250W |      0MiB / 11178MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   1  GeForce GTX 108...  Off  | 00000000:04:00.0 Off |                  N/A |
| 24%   37C    P0    61W / 250W |      0MiB / 11178MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   2  GeForce GTX 108...  Off  | 00000000:83:00.0 Off |                  N/A |
| 23%   35C    P0    60W / 250W |      0MiB / 11178MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
|   3  GeForce GTX 108...  Off  | 00000000:84:00.0 Off |                  N/A |
| 23%   31C    P0    58W / 250W |      0MiB / 11178MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+

# Test2
sudo docker run --runtime=nvidia --rm -it -p 8888:8888 tensorflow/tensorflow:latest-gpu-py3
通过浏览器访问 http://127.0.0.1:8888
```

* 卸载 nvidia-docker 1.0
```bash
# If you have nvidia-docker 1.0 installed: we need to remove it and all existing GPU containers
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
sudo apt-get purge -y nvidia-docker
```

## 下载容器（深度学习框架）
* 从[NVIDIA GPU Cloud (NGC)](https://ngc.nvidia.com/catalog/containers)下载
```bash
sudo docker pull nvcr.io/nvidia/caffe2:18.08-py3
sudo docker pull nvcr.io/nvidia/tensorflow:18.12-py3
sudo docker pull nvcr.io/nvidia/pytorch:18.12.1-py3
```

* 从[docker hub](https://hub.docker.com)下载
```bash
sudo docker pull tensorflow/tensorflow:latest-gpu-py3
sudo docker pull pytorch/pytorch:latest
sudo docker pull spellrun/caffe2:latest
```

## 参考资料
* [nvidia-docker](https://github.com/NVIDIA/nvidia-docker)
* [nvidia-docker FAQ](https://github.com/NVIDIA/nvidia-docker/wiki/Frequently-Asked-Questions#which-docker-packages-are-supported)
* [安裝 NVIDIA Docker 2 來讓容器使用 GPU](https://kairen.github.io/2018/02/17/container/docker-nvidia-install/)
* [Nvidia-Docker安装使用 -- 可使用GPU的Docker容器](https://blog.csdn.net/A632189007/article/details/78801166)
* [nvidia-docker 报错](https://blog.csdn.net/u012939880/article/details/79969601)
* [ubuntu16.04下docker和nvidia-docker安装](https://blog.csdn.net/qq_41493990/article/details/81624419)
