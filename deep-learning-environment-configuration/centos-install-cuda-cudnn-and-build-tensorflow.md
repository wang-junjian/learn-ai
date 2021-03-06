# 在CentOS上安装CUDA、cuDNN和编译TensorFlow

## 系统环境
* 操作系统：CentOS7
* 显卡：GTX 1060 6G
* CPU：八代 i5-8400
* 内存：16G
* SSD：250G

## 安装 CentOS7
* 下载 [CentOS](https://www.centos.org/download/)
* 制作U盘启动盘
    > 运行 UltraISO，[文件]->[打开]，选择CentOS映像打开；[启动光盘]->[写入硬盘映像]，弹出对话模型后，选择U盘，单击写入。
* 跟随步骤安装系统

## 安装 NVIDIA 显卡驱动和 CUDA
* 禁用默认显卡驱动 nouveau

```shell
sudo nano /usr/lib/modprobe.d/dist-blacklist.conf
    blacklist nouveau
    options nouveau modeset=0

# 备份原来的 initramfs nouveau image镜像
sudo mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -img)-r.nouveau
# 创建新的 initramfs image镜像
sudo dracut /boot/initramfs-$(uname -r).img $(uname -r)
```

* 下载 [CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit)
* 安装 CUDA Toolkit

```shell
init 3
sudo sh cuda_9.2.88_396.26_linux.run
    Do you accept the previously read EULA?
    accept/decline/quit: accept

    Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 396.26?
    (y)es/(n)o/(q)uit: y

    Do you want to install the OpenGL libraries?
    (y)es/(n)o/(q)uit [ default is yes ]: 

    Do you want to run nvidia-xconfig?
    This will update the system X configuration file so that the NVIDIA X driver
    is used. The pre-existing X configuration file will be backed up.
    This option should not be used on systems that require a custom
    X configuration, such as systems with multiple GPU vendors.
    (y)es/(n)o/(q)uit [ default is no ]: 

    Install the CUDA 9.2 Toolkit?
    (y)es/(n)o/(q)uit: y

    Enter Toolkit Location
    [ default is /usr/local/cuda-9.2 ]: 

    Do you want to install a symbolic link at /usr/local/cuda?
    (y)es/(n)o/(q)uit: y

    Install the CUDA 9.2 Samples?
    (y)es/(n)o/(q)uit: y

    Enter CUDA Samples Location
    [ default is /home/wjunjian ]: 
```

* 安装补丁包

```shell
sudo sh cuda_9.2.88.1_linux.run
```

* 配置环境变量

```shell
sudo nano /etc/profile
    export PATH=/usr/local/cuda-9.2/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda-9.2/lib64:$LD_LIBRARY_PATH
source /etc/profile
```

* 验证NVIDIA驱动安装成功

```shell
nvidia-smi
nvidia-settings
```

## 安装 cuDNN
* 下载[cuDNN](https://developer.nvidia.com/rdp/cudnn-download)
* 安装 cuDNN

```shell
tar -zxvf cudnn-9.2-linux-x64-v7.1.tgz
sudo cp cuda/include/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

## 安装 Python3
* 安装 Python3

```shell
yum search python3
sudo yum install python36
```
* 查看连接

```shell
ls -li /usr/bin/python36
2487064 lrwxrwxrwx. 1 root root 18 6月   5 22:50 /usr/bin/python36 -> /usr/bin/python3.6
```

* 创建符号连接

```shell
ln -s python3.6 python3
```

## 安装 pip3

```shell
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```

## 安装 TensorFlow GPU

```shell
pip3 install tensorflow-gpu
```

## 源码编译 TensorFlow GPU
* 下载 TensorFlow 源码
```shell
git clone https://github.com/tensorflow/tensorflow.git
```

* 安装依赖的库
```shell
yum search python3 | grep devel
sudo yum install python36-devel.x86_64

sudo yum install libibverbs-devel
```

* 安装 bazel
```shell
wget https://copr.fedorainfracloud.org/coprs/vbatts/bazel/repo/epel-7/vbatts-bazel-epel-7.repo
sudo mv vbatts-bazel-epel-7.repo /etc/yum.repos.d/
sudo yum install bazel
```

* Configure
```shell
./configure 
    WARNING: Running Bazel server needs to be killed, because the startup options are different.
    You have bazel 0.14.0- (@non-git) installed.
    Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3


    Found possible Python library paths:
    /usr/lib/python3.6/site-packages
    /usr/lib64/python3.6/site-packages
    Please input the desired Python library path to use.  Default is [/usr/lib/python3.6/site-packages]

    Do you wish to build TensorFlow with jemalloc as malloc support? [Y/n]: y
    jemalloc as malloc support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with Google Cloud Platform support? [Y/n]: n
    No Google Cloud Platform support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with Hadoop File System support? [Y/n]: y
    Hadoop File System support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with Amazon S3 File System support? [Y/n]: n
    No Amazon S3 File System support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with Apache Kafka Platform support? [Y/n]: y
    Apache Kafka Platform support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with XLA JIT support? [y/N]: y
    XLA JIT support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with GDR support? [y/N]: y
    GDR support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with VERBS support? [y/N]: y
    VERBS support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: n
    No OpenCL SYCL support will be enabled for TensorFlow.

    Do you wish to build TensorFlow with CUDA support? [y/N]: y
    CUDA support will be enabled for TensorFlow.

    Please specify the CUDA SDK version you want to use. [Leave empty to default to CUDA 9.0]: 9.2


    Please specify the location where CUDA 9.2 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 


    Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7.0]: 7.1


    Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]:


    Do you wish to build TensorFlow with TensorRT support? [y/N]: n
    No TensorRT support will be enabled for TensorFlow.

    Please specify the NCCL version you want to use. [Leave empty to default to NCCL 1.3]: 


    Please specify a list of comma-separated Cuda compute capabilities you want to build with.
    You can find the compute capability of your device at: https://developer.nvidia.com/cuda-gpus.
    Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 6.1]


    Do you want to use clang as CUDA compiler? [y/N]: n
    nvcc will be used as CUDA compiler.

    Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]: 


    Do you wish to build TensorFlow with MPI support? [y/N]: n
    No MPI support will be enabled for TensorFlow.

    Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 


    Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: n
    Not configuring the WORKSPACE for Android builds.

    Preconfigured Bazel build configs. You can use any of the below by adding "--config=<>" to your build command. See tools/bazel.rc for more details.
    --config=mkl             # Build with MKL support.
    --config=monolithic      # Config for mostly static monolithic build.
    Configuration finished
```

* Build
**编译花了1个小时，内存使用6G左右，6核满负核运行，温度在70度上下**
```shell
bazel build -c opt --config=cuda //tensorflow/tools/pip_package:build_pip_package
    ......
    Target //tensorflow/tools/pip_package:build_pip_package up-to-date:
    bazel-bin/tensorflow/tools/pip_package/build_pip_package
    INFO: Elapsed time: 3422.703s, Critical Path: 102.81s
    INFO: 9402 processes, local.
    INFO: Build completed successfully, 11829 total actions

bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

* Install
```shell
sudo pip install /tmp/tensorflow_pkg/tensorflow-1.8.0-cp36-cp36m-linux_x86_64.whl 
```

## 测试 TensorFlow GPU
* 下载 tensorflow models
```shell
git clone https://github.com/tensorflow/models.git 
```

* 训练 cifar10 （花费了4个小时）
```shell
cd models/tutoricals/image/cifar10
python3 cifar10_train.py
```
**训练 cifar10 GPU使用情况：内存不够用，GPU处理器85％左右，带宽使用了20％**
![](images/tensorflow_training_cifar10_gpu.png)

## FAQ
### 安装 CentOS7 失败。
#### 错误信息：
`Warning: /dev/root does not exist` 

#### 解决方法：

    * 查看U盘启动盘的文件名，在命令行输入：`ls /dev`，我的U盘文件名是sdb4。
        > 如果不知道的话，可以运行一次命令，然后拔下U盘，再运行一次命令，对比两次的结果，看看少了哪个文件名。
    * 重启系统，回到CentOS安装菜单，在菜单项 Install CentOS 7 按 e 键进行编辑模式，修改`linuxefi /images/vmlinuz inst.stage2=hd:LABEL=CentOS\x207\x20x86_64 quiet` 为 `linuxefi /images/vmlinuz inst.stage2=hd:/dev/sdb4 quiet`
    * 按`ctrl+x`，重启系统。

### 编译 tensorflow 失败。
#### 错误信息：
`fatal error: Python.h: No such file or directory` 

#### 解决方法：
    * 安装 python-devel
    `yum install python-devel`

### 编译 tensorflow 失败。
#### 错误信息：
`gcc failed and fatal error: infiniband/verbs.h: No such file or directory` 

#### 解决方法：
* 安装 libibverbs-devel
`yum install libibverbs-devel`

### import tensorflow 失败。
#### 错误信息：
```
    File "/usr/lib/python3.6/site-packages/tensorflow/python/keras/backend/__init__.py", line 22, in <module>    
    from tensorflow.python.keras._impl.keras.backend import abs    
    ImportError: cannot import name 'abs'
```    
#### 解决方法：
> 可能同时安装了 CPU 和 GPU 版的 TensorFlow**
    
    * 卸载 TensorFlow CPU 和 GPU 版本
```shell
    sudo pip uninstall tensorflow
    sudo pip uninstall tensorflow-gpu 
```
    * 安装 TensorFlow GPU 版本
```shell
    sudo pip install /tmp/tensorflow_pkg/tensorflow-1.8.0-cp36-cp36m-linux_x86_64.whl 
```

## 参考资料
* [Mastering Markdown](https://guides.github.com/features/mastering-markdown/)
* [Markdown语法说明（详解版）](http://www.ituring.com.cn/article/504)
* [Markdown 语法快速入门手册](https://www.w3cschool.cn/markdownyfsm/markdownyfsm-odm6256r.html)

* [U盘安装centos7解决could not boot，/dev/root/does not exit](https://blog.csdn.net/u012140170/article/details/40423645)
* [u盘安装centos7 /dev/root does not exist 导致无法安装解决方案](https://blog.csdn.net/bajiudongfeng/article/details/47732377)

* [NVIDIA cuda7在centos6.5中的安装](https://blog.csdn.net/syx19930206/article/details/47253861)
* [CentOS中禁用nouveau驱动](https://blog.csdn.net/cmzsteven/article/details/49049327)
* [centos关机与重启命令](https://www.cnblogs.com/endv/p/6622452.html)
* [CentOS 7中以runfile形式安装CUDA 9.0](https://www.cnblogs.com/alliance/p/7905657.html)
* [什么方式可以实现init3方式启动又能快速使用图形](http://tieba.baidu.com/f?kz=2065847097&mo_device=1&ssid=0&from=1000539d&uid=0&pu=usm@1,sz@1320_2001,ta@iphone_1_11.3_3_605&bd_page_type=1&baiduid=00D058CFE90959DB7374FA43FDE5A43E&tj=www_normal_3_0_10_title&referer=m.baidu.com?pn=0&&red_tag=p0293373883)

* [CentOS 7 运行级别切换](https://www.cnblogs.com/lanston/p/3880577.html)
* [lspci命令](http://man.linuxde.net/lspci)
* [TensorRT简介](https://blog.csdn.net/fengbingchun/article/details/78469551)
* [What is SYCL 1.2?](https://stackoverflow.com/questions/41831214/what-is-sycl-1-2/41877617)

* [Installing Bazel on Fedora and CentOS](https://github.com/bazelbuild/bazel/blob/master/site/docs/install-redhat.md)
* [Fedora COPR bazel](https://copr.fedorainfracloud.org/coprs/vbatts/bazel/)

* [CentOS 7.0 下 GPU 安装配置指南及 TensorFlow / Openface 的 GPU 使用](https://www.jianshu.com/p/75e7053bdd43)
* [centos7下安装部署tensorflow GPU 版本](https://cloud.tencent.com/info/be0c8667decbadc9588cd68e5289cfd3.html)
* [Installing tensorflow-gpu on CentOS 6.8](https://stackoverflow.com/questions/45081855/installing-tensorflow-gpu-on-centos-6-8)
* [tensorflow之cifar10练习](https://www.jianshu.com/p/81edb51128fc)

* [cuda和GPU驱动安装](http://blog.sina.com.cn/s/blog_710dba0d0102wfs7.html)
* [CentOS 7 Tensorflow-GPU 安装遇到的坑记录](https://www.jianshu.com/p/3d4a795e7b69)
* [Centos 安装cuda](https://blog.csdn.net/zijin0802034/article/details/54575067)
* [双显卡独显驱动问题](https://bbs.deepin.org/forum.php?mod=viewthread&tid=136564)
* [centos 7 下安装cuDNN](https://blog.csdn.net/yaoqiwaimai/article/details/78594154)
* [CentOS yum的详细使用方法](https://www.cnblogs.com/xuange306/p/6049481.html)
* [centos 7 yum 安装 python3](https://www.cnblogs.com/elodio/p/4900160.html)
* [CentOS7安装EPEL的两种方式](http://www.mamicode.com/info-detail-1671603.html)
* [Centos7下python3安装pip-9.0.1](https://www.cnblogs.com/chengd/p/7078588.html)
* [centos6下同时安装python2和python3](https://www.cnblogs.com/lawlietfans/p/8613974.html)
* [ImportError: libcublas.so.9.0: cannot open shared object file: No such file or directory when installing tensor flow on Ubuntu 16.04.03](https://stackoverflow.com/questions/49115391/importerror-libcublas-so-9-0-cannot-open-shared-object-file-no-such-file-or-d)
* [tensorflow-gpu版本的编译](http://windywinter.cn/2018/03/10/ML/tensorflow-gpu%E7%89%88%E6%9C%AC%E7%9A%84%E7%BC%96%E8%AF%91/)
* [安装tensorflow，那叫一个坑啊](https://www.cnblogs.com/shihuc/p/6593041.html)
* [使用bazel 编译tensorflow serving时报错：fatal error: Python.h: No such file or directory](https://www.aliyun.com/jiaocheng/123469.html)
* [What is Bazel in TensorFlow? When do I need to build again?](https://stackoverflow.com/questions/35422388/what-is-bazel-in-tensorflow-when-do-i-need-to-build-again)
* [tensorflow 之 bazel安装 & 使用](https://blog.csdn.net/u010700335/article/details/69422282)
* [How to install Tensorflow GPU with CUDA Toolkit 9.1 and cuDNN 7.1.2 for Python 3 on Ubuntu 16.04-64bit](http://www.python36.com/install-tensorflow141-gpu/)
* [NVIDIA TensorRT使用记录](https://www.jianshu.com/p/59fe26073a41)
* [NVIDIA TensorRT Download](https://developer.nvidia.com/nvidia-tensorrt-download)
* [How to install python3-devel on red hat 7](https://stackoverflow.com/questions/43047284/how-to-install-python3-devel-on-red-hat-7)
* [gcc failed and fatal error: infiniband/verbs.h: No such file or directory](https://github.com/tensorflow/tensorflow/issues/9966)
* [深度学习利器: TensorFlow系统架构及高性能程序设计](http://www.sohu.com/a/136025802_116235)
* [CentOS查看CPU温度](https://blog.csdn.net/jajavaja/article/details/48212009)
* [how to import function from models.py to run a cron job?](https://stackoverflow.com/questions/34939305/how-to-import-function-from-models-py-to-run-a-cron-job)
* [TensorFlow学习一：源码安装](https://blog.csdn.net/u013832707/article/details/73161071)
* [确定自己的TensorFlow是CPU还是GPU的版本](https://blog.csdn.net/zlase/article/details/79261348)
* [How to test tensorflow cifar10 cnn tutorial model](https://stackoverflow.com/questions/33784214/how-to-test-tensorflow-cifar10-cnn-tutorial-model)
* [TensorFlow CNN 测试CIFAR-10数据集](https://blog.csdn.net/yhl_leo/article/details/50738311)
* [利用tensorflow搭建CIFAR10 分类网络](https://blog.csdn.net/CZ626626/article/details/65627598)
* [TensorFlow指定GPU使用及监控GPU占用情况](https://www.cnblogs.com/helloyy/p/7878201.html)

* [深度学习指南：基于Ubuntu从头开始搭建环境](https://blog.csdn.net/happytofly/article/details/80123546)
