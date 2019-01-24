# 源代码编译TensorFlow GPU

## 下载 TensorFlow 源码
```shell
git clone https://github.com/tensorflow/tensorflow.git
```

## 安装 Bazel
* Ubuntu
```bash
sudo apt install curl

sudo apt-get install openjdk-8-jdk

echo “deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8” | sudo tee /etc/apt/sources.list.d/bazel.list
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -

sudo apt-get update
sudo apt-get install bazel
```

* CentOS
```bash
yum search python3 | grep devel
sudo yum install python36-devel.x86_64
sudo yum install libibverbs-devel

wget https://copr.fedorainfracloud.org/coprs/vbatts/bazel/repo/epel-7/vbatts-bazel-epel-7.repo
sudo mv vbatts-bazel-epel-7.repo /etc/yum.repos.d/
sudo yum install bazel
```

## 安装 NCCL
* 下载 [NCCL](https://developer.nvidia.com/nccl)
* 安装
```bash
sudo dpkg -i nccl-repo-ubuntu1804-2.3.7-ga-cuda10.0_1-1_amd64.deb
sudo dpkg -i /var/nccl-repo-2.3.7-ga-cuda10.0/libnccl2_2.3.7-1+cuda10.0_amd64.deb
sudo dpkg -i /var/nccl-repo-2.3.7-ga-cuda10.0/libnccl-dev_2.3.7-1+cuda10.0_amd64.deb
```

## 编译 TensorFlow GPU
* Configure
```bash
./configure
WARNING: Running Bazel server needs to be killed, because the startup options are different.
WARNING: --batch mode is deprecated. Please instead explicitly shut down your Bazel server using the command "bazel shutdown".
INFO: Invocation ID: 2d9acee5-8bac-47ca-b9ee-0b0bfc693b16
You have bazel 0.21.0 installed.
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3


Found possible Python library paths:
  /usr/lib/python3/dist-packages
  /usr/local/lib/python3.6/dist-packages
Please input the desired Python library path to use.  Default is [/usr/lib/python3/dist-packages]

Do you wish to build TensorFlow with XLA JIT support? [Y/n]: n
No XLA JIT support will be enabled for TensorFlow.

Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: n
No OpenCL SYCL support will be enabled for TensorFlow.

Do you wish to build TensorFlow with ROCm support? [y/N]: n
No ROCm support will be enabled for TensorFlow.

Do you wish to build TensorFlow with CUDA support? [y/N]: y
CUDA support will be enabled for TensorFlow.

Please specify the CUDA SDK version you want to use. [Leave empty to default to CUDA 10.0]: 


Please specify the location where CUDA 10.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 


Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7]: 


Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 


Do you wish to build TensorFlow with TensorRT support? [y/N]: n
No TensorRT support will be enabled for TensorFlow.

Please specify the locally installed NCCL version you want to use. [Default is to use https://github.com/nvidia/nccl]: 


Please specify a list of comma-separated Cuda compute capabilities you want to build with.
You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 6.1,6.1,6.1,6.1]: 


Do you want to use clang as CUDA compiler? [y/N]: n
nvcc will be used as CUDA compiler.

Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: 


Do you wish to build TensorFlow with MPI support? [y/N]: n
No MPI support will be enabled for TensorFlow.

Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native -Wno-sign-compare]: 


Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: n
Not configuring the WORKSPACE for Android builds.

Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See .bazelrc for more details.
	--config=mkl         	# Build with MKL support.
	--config=monolithic  	# Config for mostly static monolithic build.
	--config=gdr         	# Build with GDR support.
	--config=verbs       	# Build with libverbs support.
	--config=ngraph      	# Build with Intel nGraph support.
	--config=dynamic_kernels	# (Experimental) Build kernels into separate shared objects.
Preconfigured Bazel build configs to DISABLE default on features:
	--config=noaws       	# Disable AWS S3 filesystem support.
	--config=nogcp       	# Disable GCP support.
	--config=nohdfs      	# Disable HDFS support.
	--config=noignite    	# Disable Apache Ignite support.
	--config=nokafka     	# Disable Apache Kafka support.
	--config=nonccl      	# Disable NVIDIA NCCL support.
Configuration finished
```

* Build
```bash
bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

## 安装 TensorFlow GPU
```bash
sudo pip3 install /tmp/tensorflow_pkg/tensorflow-1.12.0-cp36-cp36m-linux_x86_64.whl
```


## 参考资料
* [Installing Bazel](https://docs.bazel.build/versions/master/install.html)
* [Installing Bazel on Ubuntu](https://docs.bazel.build/versions/master/install-ubuntu.html#install-on-ubuntu)
* [Install CUDA 10.0, cuDNN 7.3 and build TensorFlow (GPU) from source on Ubuntu 18.04](https://medium.com/@vitali.usau/install-cuda-10-0-cudnn-7-3-and-build-tensorflow-gpu-from-source-on-ubuntu-18-04-3daf720b83fe)
* [Linux中的configure,make,make install到底在做些什么](http://blog.51cto.com/lee90/1969632)
* [理解 configure 脚本](https://zhuanlan.zhihu.com/p/32369152)