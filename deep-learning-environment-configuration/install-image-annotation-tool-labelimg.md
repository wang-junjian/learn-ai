# 安装图像标注工具 LabelImg

## 安装
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
