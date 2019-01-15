# 安装图像标注工具 LabelImg

## 安装
* MacOS
```bash
brew install qt
brew install libxml2

git clone https://github.com/tzutalin/labelImg.git
cd labelImg
make qt5py3
```

* CentOS7
```bash
wget http://download.qt.io/archive/qt/5.12/5.12.0/qt-opensource-linux-x64-5.12.0.run
chmod +x qt-opensource-linux-x64-5.12.0.run
./qt-opensource-linux-x64-5.12.0.run

sudo pip3 install pyqt5

git clone https://github.com/tzutalin/labelImg.git
cd labelImg
```

## 运行
```bash
python3 labelImg.py

# python3 labelImg.py [图像目录] [分类文件]
python3 labelImg.py input/ class_list.txt
```

## FAQ
* AssertionError: Missing string id : useDefaultLabel
```bash
cd labelImg
make qt5py3
````

## 参考资料
* [labelImg](https://github.com/tzutalin/labelImg)
* [Mac 环境下labelImg标注工具的安装](https://blog.csdn.net/fufeng0522/article/details/79690763)
* [图像标注工具labelImg使用方法](https://www.cnblogs.com/Terrypython/p/9577657.html)
* [LabelImg图片标注](https://www.jianshu.com/p/8d78362fe9cf)
* [在CentOS 7.3(1611)上安装Qt 5.8.0开发环境](https://segmentfault.com/a/1190000008476086)
* [Qt5.12.0](http://download.qt.io/archive/qt/5.12/5.12.0/)
* [Installing PyQt5](http://pyqt.sourceforge.net/Docs/PyQt5/installation.html)
