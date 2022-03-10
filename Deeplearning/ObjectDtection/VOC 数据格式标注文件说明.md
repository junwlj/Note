# VOC标注文件数据说明

### 整体目录架构

PASCAL VOC数据集由5个部分构成：JPEGImages，Annotations，ImageSets，SegmentationClass以及SegmentationObject。

- **JPEGImages**：存放的是训练与测试的所有图片。
- **Annotations**：里面存放的是每张图片打完标签所对应的XML文件
- **ImageSets**：ImageSets文件夹下本次讨论的只有Main文件夹，此文件夹中存放的主要又有四个文本文件**test.txt**,**train.txt**,**trainval.txt**,**val.txt**,其中分别存放的是**测试集**图片的文件名、**训练集**图片的文件名、**训练验证集**图片的文件名、**验证集**图片的文件名。
- **SegmentationClass与SegmentationObject**：存放的都是图片，且都是图像分割结果图，对目标检测任务来说没有用。**class segmentation** 标注出每一个像素的类别 。**object segmentation** 标注出每一个像素属于哪一个物体

#### XML文件标注说明

```xml
<annotation>
  <folder>17</folder> # 图片所处文件夹
  <filename>77258.bmp</filename> # 图片名
  <path>~/frcnn-image/61/ADAS/image/frcnn-image/17/77258.bmp</path>
  <source>  #图片来源相关信息
    <database>Unknown</database>  
  </source>
  <size> #图片尺寸
    <width>640</width>
    <height>480</height>
    <depth>3</depth>
  </size>
  <segmented>0</segmented>  #是否有分割label
  <object> 包含的物体
    <name>car</name>  #物体类别
    <pose>Unspecified</pose>  #物体的姿态
    <truncated>0</truncated>  #物体是否被部分遮挡（>15%）
    <difficult>0</difficult>  #是否为难以辨识的物体， 主要指要结体背景才能判断出类别的物体。虽有标注， 但一般忽略这类物体
    <bndbox>  #物体的bound box
      <xmin>2</xmin>
      <ymin>156</ymin>
      <xmax>111</xmax>
      <ymax>259</ymax>
    </bndbox>
  </object>
</annotation>
```

#### label中的`.txt`文件标注说明

```python
0 0.26555023923444976 0.20461538461538462 0.16746411483253587 0.16307692307692306
# 0: 该图的类别编号
# 0.26555023923444976 归一化后的中心点x坐标
# 0.20461538461538462 归一化后中心点y坐标
# 0.16746411483253587 归一化后的目标框宽度w
# 0.16307692307692306 归一化后的目标框高度h
```

