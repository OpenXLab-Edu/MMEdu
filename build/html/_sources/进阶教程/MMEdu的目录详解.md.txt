# MMEdu的目录详解

MMEdu一键安装版是一个压缩包，解压后即可使用。

MMEdu的根目录结构如下：

```plain
OpenMMLab-Edu
├── MMEdu
├── checkpoints
├── dataset
├── demo
├── HowToStart
├── tools（github)
├── visualization（github)
├── base.py
├── README.md
├── README.pdf
├── setup.bat
├── 安装文件说明.ipynb
├── pyzo.exe
├── run_jupyter.bat
```

接下来对每层子目录进行介绍。

#### MMEdu目录：

存放各个模块的底层代码、算法模型文件夹“models”和封装环境文件夹“mmedu”。“models”文件夹中提供了各个模块常见的网络模型，内置模型配置文件和说明文档，说明文档提供了模型简介、特点、预训练模型下载链接和适用领域等。“mmedu”文件夹打包了MMEdu各模块运行所需的环境和中小学课程常用的库。

#### checkpoints目录：

存放各个模块的预训练模型的权重文件，分别放在以模块名称命名的文件夹下，如“cls_model”。

#### dataset目录：

存放为各个模块任务准备的数据集，分别放在以模块名称命名的文件夹下，如“cls”。同时github上此目录下还存放了各个模块自定义数据集的说明文档，如“pose-dataset.md”，文档提供了每个模块对应的数据集格式、下载链接、使用说明、自制数据集流程。

#### demo目录：

存放各个模块的测试程序，如“cls_demo.py”，并提供了测试图片。测试程序包括`py`文件和`ipynb`文件，可支持各种“Python IDE”和“jupyter notebook”运行，可运行根目录的“pyzo.exe”和“run_jupyter.bat”后打开测试程序。

#### HowToStart目录：

存放各个模块的使用教程文档，如“MMClassfication使用教程.md”，文档提供了代码详细说明、参数说明与使用等。同时github上此目录下还存放了OpenMMLab各个模块的开发文档供感兴趣的老师和同学参考，如“OpenMMLab_MMClassification.md”，提供了模块介绍、不同函数使用、深度魔改、添加网络等。

#### tools目录：

存放数据集格式的转换、不同框架的部署等通用工具。后续会陆续开发数据集查看工具、数据集标注工具等工具。

#### visualization目录：

存放可视化界面。
