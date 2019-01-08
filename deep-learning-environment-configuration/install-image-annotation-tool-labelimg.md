# 安装图像标注工具 LabelImg

## 安装
* MacOS

```bash
brew install qt
brew install libxml2

git clone https://github.com/tzutalin/labelImg.git
cd labelImg
make qt5py3

python3 labelImg.py
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