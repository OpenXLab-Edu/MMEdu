# 体验MMEdu

## 1.运行Demo代码

1）使用默认IDE

双击pyzo.exe，打开demo文件夹中的cls_demo.py，运行并体验相关功能，也可以查看其他的Demo文件。详细说明可以在HowToStart文件夹看到。

2）使用第三方IDE

环境支持任意的Python编辑器，如：Thonny、PyCharm、Sublime等。
只要配置其Python解释器地址为`{你的安装目录}+{\MMEdu\mmedu\python.exe}`。



## 2.体验快速入门课程

体验入门Demo后，我们还准备了一系列的入门课程供您参考。将在稍晚发布。



## 3.MMEdu的基本代码风格
推理：
```python
from MMEdu import MMClassification as cls
img = './img.png'
model = cls(backbone='LeNet')
checkpoint = './latest.pth'
class_path = './classes.txt'
result = model.inference(image=img, show=True, class_path=class_path,checkpoint = checkpoint)
model.print_result(result)
```
典型训练：
```python
from MMEdu import MMClassification as cls
model = cls(backbone='LeNet')
model.num_classes = 3
model.load_dataset(path='./dataset')
model.save_fold = './my_model'
model.train(epochs=10, validate=True)
```
继续训练：
```python
from MMEdu import MMClassification as cls
model = cls(backbone='LeNet')
model.num_classes = 3
model.load_dataset(path='./dataset')
model.save_fold = './my_model'
checkpoint = './latest.pth'
model.train(epochs=10, validate=True, checkpoint=checkpoint)
```
