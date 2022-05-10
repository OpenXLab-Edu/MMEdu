# Web库PyWebIO简介

## 1.简介

顾名思义，PyWebIO库是一个基于Web方式来实现输入输出（I/O）的Python库。这是北京航空航天大学在读研究生王伟民同学用业余时间写的库。目前在GitHbu上获得了高达1.6K的Star。它允许用户像编写终端脚本一样来编写Web应用或基于浏览器的GUI应用，而无需具备HTML和JS的相关知识。

Github地址：https://github.com/wang0618/PyWebIO

## 2. 安装

PyWebIO可以采用pip命令安装，具体如下：

```
pip install PyWebIO
```

注：MMEdu中已经内置了PyWebIO库。

## 3. 代码示例

PyWebIO提供了一系列命令式的交互函数来在浏览器上获取用户输入和进行输出，相当于将浏览器变成了一个“富文本终端”。如：

```python
from pywebio.input import *
from pywebio.output import *
# 文本输入
s = input('请输入你的名字：')
# 输出文本
put_text('欢迎你，' + s);
```

运行这段代码后，浏览器会自动打开一个本地的网址，出现这样的界面。

![img](https://cdn.nlark.com/yuque/0/2022/png/2493078/1650379473302-ad990eb0-63fe-4858-80ae-f9a21a229e33.png)

​                                                                        图1 初始网页界面

输入姓名再点击“提交”按钮，网页上就会输出相应的文字。这种基于Web页面的“交互”，体验比黑乎乎的终端界面要好很多。

PyWebIO支持常见的网页控件。既然PyWebI的定位就是输入和输出，那么也可以将网页控件分为这两类，部分控件的说明如表1所示。

表1 PyWebIO支持的网页控件（部分）

| 类别     | 控件                                                         | 代码范例                   |
| -------- | ------------------------------------------------------------ | -------------------------- |
| 输入     | 文本                                                         | input("What's your name?") |
| 下拉选择 | select('Select', ['A', 'B'])                                 |                            |
| 多选     | checkbox("Checkbox", options=['Check me'])                   |                            |
| 单选     | radio("Radio", options=['A', 'B', 'C'])                      |                            |
| 多行文本 | textarea('Text', placeholder='Some text')                    |                            |
| 文件上传 | file_upload("Select a file:")                                |                            |
| 输出     | 文本                                                         | put_text("Hello world!");  |
| 表格     | put_table([['Product', 'Price'],['Apple', '$5.5'], ['Banner', '$7'],]); |                            |
| 图像     | put_image(open('python-logo.png', 'rb').read());             |                            |
| 通知消息 | toast('Awesome PyWebIO!!');                                  |                            |
| 文件     | put_file('hello_word.txt', b'hello word!');                  |                            |
| Html代码 | put_html('E = mc<sup>2</sup>');                              |                            |



尤其值得称赞的是，PyWebIO还支持MarkDown语法。除了输入输出，PyWebIO还支持布局、协程、数据可视化等特性。通过和其他库的配合，可以呈现更加酷炫的网页效果，如图2所示。

![img](https://cdn.nlark.com/yuque/0/2022/png/2493078/1650379764579-26c89f1b-109b-4fa9-9e24-6b1c9510fc52.png)

​                                           图2 PyWebIO结合第三方库制作的数据可视化效果

如果需要了解更多关于PyWebIO库的资源，请访问github或者官方文档。

文档地址：https://pywebio.readthedocs.io/

## 4. 借助PyWebIO部署简易AI应用

在人工智能教学过程中，我们常常为模型的部署而烦恼。如果训练出来的模型不能有效应用于生活，或者解决一些真实问题，则很难打动学生，激发学习兴趣。

PyWebIO能够将AI模型快速“变身”为Web应用，上传一张照片就能输出识别结果，极大地提高了学生的学习收获感。