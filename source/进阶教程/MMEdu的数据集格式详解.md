# MMEdu的数据集格式详解

MMEdu系列提供了包括分类、检测等任务的若干数据集，存储在dataset文件夹下。

## 1.ImageNet

ImageNet是斯坦福大学提出的一个用于视觉对象识别软件研究的大型可视化数据库，目前大部分模型的性能基准测试都在ImageNet上完成。MMEdu的MMClassification支持的数据集类型是ImageNet，如需训练自己创建的数据集，数据集需转换成ImageNet格式。

ImageNet格式数据集文件夹结构如下所示，图像数据文件夹和标签文件放在同级目录下。

```plain
imagenet
├── ...
├── training_set
│   ├── class_0
│   │   ├── filesname_0.JPEG
│   │   ├── filesname_1.JPEG
│   │   ├── ...
│   ├── ...
│   ├── class_n
│   │   ├── filesname_0.JPEG
│   │   ├── filesname_1.JPEG
│   │   ├── ...
├── classes.txt
├── ...
```

如上所示训练数据根据图片的类别，存放至不同子目录下，子目录名称为类别名称。

classes.txt包含数据集类别标签信息，每行包含一个类别名称，按照字母顺序排列。

```plain
class_0
class_1
...
class_n
```

为了验证和测试，我们建议划分训练集、验证集和测试集，此时需另外生成“val.txt”和“test.txt”这两个标签文件，要求是每一行都包含一个文件名和其相应的真实标签。格式如下所示：

```plain
filesname_0.jpg 0
filesname_1.jpg 0
...
filesname_a.jpg n
filesname_b.jpg n
```

注：真实标签的值应该位于`[0,类别数目-1]`之间。

这里，为您提供一段用Python代码完成标签文件的程序如下所示，程序中设计了“val.txt”和“test.txt”这两个标签文件每行会包含类别名称、文件名和真实标签。

```plain
import os
# 列出指定目录下的所有文件名，确定类别名称
classes = os.listdir('D:\测试数据集\EX_dataset\\training_set')
# 打开指定文件，并写入类别名称
with open('D:\测试数据集\EX_dataset/classes.txt','w') as f:
    for line in classes:
        str_line = line +'\n'
        f.write(str_line) # 文件写入str_line，即类别名称

test_dir = 'D:\测试数据集\EX_dataset\\test_set/' # 指定测试集文件路径
# 打开指定文件，写入标签信息
with open('D:\测试数据集\EX_dataset/test.txt','w') as f:
    for cnt in range(len(classes)):
        t_dir = test_dir + classes[cnt]  # 指定测试集某个分类的文件目录
        files = os.listdir(t_dir) # 列出当前类别的文件目录下的所有文件名
        # print(files)
        for line in files:
            str_line = classes[cnt] + '/' + line + ' '+str(cnt) +'\n' 
            f.write(str_line) 

val_dir = 'D:\测试数据集\EX_dataset\\val_set/'  # 指定文件路径
# 打开指定文件，写入标签信息
with open('D:\测试数据集\EX_dataset/val.txt', 'w') as f:
    for cnt in range(len(classes)):
        t_dir = val_dir + classes[cnt]  # 指定验证集某个分类的文件目录
        files = os.listdir(t_dir)  # 列出当前类别的文件目录下的所有文件名
        # print(files)
        for line in files:
            str_line = classes[cnt] + '/' + line + ' ' + str(cnt) + '\n'
            f.write(str_line)  # 文件写入str_line，即标注信息
```

至于如何从零开始制作一个ImageNet格式的数据集，可参考如下步骤。

#### 第一步：整理图片

您可以用任何设备拍摄图像，也可以从视频中抽取帧图像，需要注意，这些图像可以被划分为多个类别。每个类别建立一个文件夹，文件夹名称为类别名称，将图片放在其中。

接下来需要对图片进行尺寸、保存格式等的统一，可使用如下代码：

```plain
from PIL import Image
from torchvision import transforms
import os

def makeDir(folder_path):
    if not os.path.exists(folder_path):  # 判断是否存在文件夹如果不存在则创建为文件夹
        os.makedirs(folder_path)

classes = os.listdir('D:\测试数据集\自定义数据集')
read_dir = 'D:\测试数据集\自定义数据集/' # 指定原始图片路径
new_dir = 'D:\测试数据集\自定义数据集new/'
for cnt in range(len(classes)):
    r_dir = read_dir + classes[cnt] + '/'
    files = os.listdir(r_dir)
    for index,file in enumerate(files):
        img_path = r_dir + file
        img = Image.open(img_path)   # 读取图片
        resize = transforms.Resize([224, 224])
        IMG = resize(img)
        w_dir = new_dir + classes[cnt] + '/'
        makeDir(w_dir)
        save_path = w_dir + str(index)+'.jpg'
        IMG = IMG.convert('RGB')
        IMG.save(save_path)
```

#### 第二步：划分训练集、验证集和测试集

根据整理的数据集大小，按照一定比例拆分训练集、验证集和测试集，可使用如下代码将原始数据集按照“6:2:2”的比例拆分。

```plain
import os
import shutil
# 列出指定目录下的所有文件名，确定类别名称
classes = os.listdir('D:\测试数据集\自定义表情数据集')

# 定义创建目录的方法
def makeDir(folder_path):
    if not os.path.exists(folder_path):  # 判断是否存在文件夹如果不存在则创建为文件夹
        os.makedirs(folder_path)

# 指定文件目录
read_dir = 'D:\测试数据集\自定义表情数据集/' # 指定原始图片路径
train_dir = 'D:\测试数据集\自制\EX_dataset\\training_set/' # 指定训练集路径
test_dir = 'D:\测试数据集\自制\EX_dataset\\test_set/' # 指定测试集路径
val_dir = 'D:\测试数据集\自制\EX_dataset\\val_set/' # 指定验证集路径

for cnt in range(len(classes)):
    r_dir = read_dir + classes[cnt] + '/'  # 指定原始数据某个分类的文件目录
    files = os.listdir(r_dir)  # 列出某个分类的文件目录下的所有文件名
    files = files[:1000]
    # 按照6:2:2拆分文件名
    offset1 = int(len(files) * 0.6)
    offset2 = int(len(files) * 0.8)
    training_data = files[:offset1]
    val_data = files[offset1:offset2]
    test_data = files[offset2:]

    # 根据拆分好的文件名新建文件目录放入图片
    for index,fileName in enumerate(training_data):
        w_dir = train_dir + classes[cnt] + '/'  # 指定训练集某个分类的文件目录
        makeDir(w_dir)
        shutil.copy(r_dir + fileName,w_dir + classes[cnt] + str(index)+'.jpg')
    for index,fileName in enumerate(test_data):
        w_dir = test_dir + classes[cnt] + '/'  # 指定测试集某个分类的文件目录
        makeDir(w_dir)
        shutil.copy(r_dir + fileName, w_dir + classes[cnt] + str(index) + '.jpg')
    for index,fileName in enumerate(val_data):
        w_dir = val_dir + classes[cnt] + '/'  # 指定验证集某个分类的文件目录
        makeDir(w_dir)
        shutil.copy(r_dir + fileName, w_dir + classes[cnt] + str(index) + '.jpg')
```

#### 第三步：生成标签文件

划分完训练集、验证集和测试集，我们需要生成“classes.txt”，“val.txt”和“test.txt”，使用上文介绍的Python代码完成标签文件的程序生成标签文件。

#### 第四步：给数据集命名

最后，我们将这些文件放在一个文件夹中，命名为数据集的名称。这样，在训练的时候，只要通过`model.load_dataset`指定数据集的路径就可以了。

## 2.COCO

COCO数据集是微软于2014年提出的一个大型的、丰富的检测、分割和字幕数据集，包含33万张图像，针对目标检测和实例分割提供了80个类别的物体的标注，一共标注了150万个物体。MMEdu的MMDetection支持的数据集类型是COCO，如需训练自己创建的数据集，数据集需转换成COCO格式。

MMEdu的MMDetection设计的COCO格式数据集文件夹结构如下所示，“annotations”文件夹存储标注文件，“images”文件夹存储用于训练、验证、测试的图片。

```plain
coco
├── annotations
│   ├── train.json
│   ├── ...
├── images
│   ├── train
│   │   ├── filesname_0.JPEG
│   │   ├── filesname_1.JPEG
│   │   ├── ...
│   ├── ...
```

如果您的文件夹结构和上方不同，则需要在“Detection_Edu.py”文件中修改`load_dataset`方法中的数据集和标签加载路径。

COCO数据集的标注信息存储在“annotations”文件夹中的`json`文件中，需满足COCO标注格式，基本数据结构如下所示。

```plain
# 全局信息
{
    "images": [image],
    "annotations": [annotation],
    "categories": [category]
}

# 图像信息标注，每个图像一个字典
image {
    "id": int,  # 图像id编号，可从0开始
    "width": int, # 图像的宽
    "height": int,  # 图像的高
    "file_name": str, # 文件名
}

# 检测框标注，图像中所有物体及边界框的标注，每个物体一个字典
annotation {
    "id": int,  # 注释id编号
    "image_id": int,  # 图像id编号
    "category_id": int,   # 类别id编号
    "segmentation": RLE or [polygon],  # 分割具体数据，用于实例分割
    "area": float,  # 目标检测的区域大小
    "bbox": [x,y,width,height],  # 目标检测框的坐标详细位置信息
    "iscrowd": 0 or 1,  # 目标是否被遮盖，默认为0
}

# 类别标注
categories [{
    "id": int, # 类别id编号
    "name": str, # 类别名称
    "supercategory": str, # 类别所属的大类，如哈巴狗和狐狸犬都属于犬科这个大类
}]
```

​	这里，为您提供一种自己制作COCO格式数据集的方法。

#### 第一步、整理图片

根据需求按照自己喜欢的方式收集图片，图片中包含需要检测的信息即可，可以使用ImageNet格式数据集整理图片的方式对收集的图片进行预处理。

#### 第二步、标注图片

可使用LabelMe批量打开图片文件夹的图片，进行标注并保存为json文件。

- LabelMe：格式为LabelMe，提供了转VOC、COCO格式的脚本，可以标注矩形、圆形、线段、点。标注语义分割、实例分割数据集尤其推荐。
- LabelMe安装与打开方式：`pip install labelme`安装完成后输入`labelme`即可打开。

#### 第三步、转换成COCO标注格式

将LabelMe格式的标注文件转换成COCO标注格式，可以使用如下代码：

```plain
import json
import numpy as np
import glob
import PIL.Image
from shapely.geometry import Polygon

class labelme2coco(object):
    def __init__(self, labelme_json=[], save_json_path='./new.json'):
        '''
        :param labelme_json: 所有labelme的json文件路径组成的列表
        :param save_json_path: json保存位置
        '''
        self.labelme_json = labelme_json
        self.save_json_path = save_json_path
        self.annotations = []
        self.images = []
        self.categories = [{'supercategory': None, 'id': 1, 'name': 'plate'}]
        self.label = []
        self.annID = 1
        self.height = 0
        self.width = 0
        self.save_json()

    # 定义读取图像标注信息的方法
    def image(self, data, num):
        image = {}
        height = data['imageHeight']
        width = data['imageWidth']
        image['height'] = height
        image['width'] = width
        image['id'] = num + 1
        image['file_name'] = data['imagePath'].split('/')[-1]
        self.height = height
        self.width = width
        return image

    # 定义数据转换方法
    def data_transfer(self):
        for num, json_file in enumerate(self.labelme_json):
            with open(json_file, 'r') as fp:
                data = json.load(fp)  # 加载json文件
                self.images.append(self.image(data, num)) # 读取所有图片标注信息并加入images数组
                for shapes in data['shapes']:
                    label = shapes['label']
                    points = shapes['points']
                    # print(points)
                    self.annotations.append(self.annotation(points, label, num)) # 读取所有检测框标注信息并加入annotations数组
                    self.annID += 1
        print(self.annotations)

    # 定义读取检测框标注信息的方法
    def annotation(self, points, label, num):
        annotation = {}
        annotation['segmentation'] = [list(np.asarray(points).flatten())]
        poly = Polygon(points)
        area_ = round(poly.area, 6)
        annotation['area'] = area_
        annotation['iscrowd'] = 0
        annotation['image_id'] = num + 1
        annotation['bbox'] = list(map(float, self.getbbox(points)))
        annotation['category_id'] = self.getcatid(label)
        annotation['id'] = self.annID
        return annotation

    # 定义读取检测框的类别信息的方法
    def getcatid(self, label):
        for categorie in self.categories:
            if label == categorie['name']:
                return categorie['id']
        return -1

    def getbbox(self, points):
        polygons = points
        mask = self.polygons_to_mask([self.height, self.width], polygons)
        return self.mask2box(mask)

    def mask2box(self, mask):
        '''从mask反算出其边框
        mask：[h,w]  0、1组成的图片
        1对应对象，只需计算1对应的行列号（左上角行列号，右下角行列号，就可以算出其边框）
        '''
        # np.where(mask==1)
        index = np.argwhere(mask == 1)
        rows = index[:, 0]
        clos = index[:, 1]
        # 解析左上角行列号
        left_top_r = np.min(rows)  # y
        left_top_c = np.min(clos)  # x

        # 解析右下角行列号
        right_bottom_r = np.max(rows)
        right_bottom_c = np.max(clos)

        return [left_top_c, left_top_r, right_bottom_c - left_top_c,
                right_bottom_r - left_top_r]  # [x1,y1,w,h] 对应COCO的bbox格式

    def polygons_to_mask(self, img_shape, polygons):
        mask = np.zeros(img_shape, dtype=np.uint8)
        mask = PIL.Image.fromarray(mask)
        xy = list(map(tuple, polygons))
        PIL.ImageDraw.Draw(mask).polygon(xy=xy, outline=1, fill=1)
        mask = np.array(mask, dtype=bool)
        return mask

    def data2coco(self):
        data_coco = {}
        data_coco['images'] = self.images
        data_coco['categories'] = self.categories
        data_coco['annotations'] = self.annotations
        return data_coco

    def save_json(self):
        self.data_transfer()
        self.data_coco = self.data2coco()
        # 保存json文件
        json.dump(self.data_coco, open(self.save_json_path, 'w'), indent=4)  # 写入指定路径的json文件，indent=4 更加美观显示

labelme_json = glob.glob('D:\测试数据集\det自定义/plate/*.json')  # 获取指定目录下的json格式的文件
labelme2coco(labelme_json, 'D:\测试数据集\det自定义/platecoco.json'
```

#### 第四步、按照目录结构整理文件

创建两个文件夹“images”和“annotations”，分别用于存放图片以及标注信息。按照要求的目录结构，整理好文件夹的文件，最后将文件夹重新命名，在训练的时候，只要通过`model.load_dataset`指定数据集的路径就可以了。