###  解析XML/CSV文件，生成json格式

本文以解析xml文件为例，通过使用`xml`包中的`etree.ElementTree`解析出`xml`文件内容，并写入到`json`文件当中，生成`json`文件格式为：

```python
{
    'images': [], # 存储images信息
    'annotations': [],  # 存储标注数据
    'categories': [] # 存储类别数据
}
```

具体解析代码如下所示

```python
import os
import cv2
import json
import xml.dom.minidom
import xml.etree.ElementTree as ET
os.environ['CUDA_VISIBLE_DEVICES']='1'

warnings.filterwarnings('ignore')

train_dir = 'G:match/JS_match/car/train'
test_dir = 'G:match/JS_match/car/test'

img_dir = os.path.join(train_dir, 'image')
xml_dir = os.path.join(train_dir, 'annotations')

annotations = {'images': [], 'annotations': [], 'categories': []}
categories_map = {'Crack': 1, 'Net': 2, 'AbnormalManhole': 3, 'Pothole': 4, 'Marking': 5}
for key in categories_map:
    categoriy_info = {"id":categories_map[key], "name":key}
    annotations['categories'].append(categoriy_info)

files = [image_file_name.split('.')[0] for image_file_name in os.listdir(img_dir)]
xmls = [image_file_name.split('.')[0] for image_file_name in os.listdir(xml_dir)]
file_names = [i for i in files if i in xmls]
ann_id = 1
for i, file_name in enumerate(file_names):
    image_file_name = file_name + '.jpg'
    xml_file_name = file_name + '.xml'
    image_file_path = os.path.join(img_dir, image_file_name)
    xml_file_path = os.path.join(xml_dir, xml_file_name)
    image_info = dict()
    image = cv2.cvtColor(cv2.imread(image_file_path), cv2.COLOR_BGR2RGB)
    height, width, _ = image.shape
    image_info = {'file_name': image_file_name, 'id': i+1,
                  'height': height, 'width': width}
    annotations['images'].append(image_info)

    DOMTree = xml.dom.minidom.parse(xml_file_path)
    collection = DOMTree.documentElement

    names = collection.getElementsByTagName('name')
    names = [name.firstChild.data for name in names]

    xmins = collection.getElementsByTagName('xmin')
    xmins = [xmin.firstChild.data for xmin in xmins]
    ymins = collection.getElementsByTagName('ymin')
    ymins = [ymin.firstChild.data for ymin in ymins]
    xmaxs = collection.getElementsByTagName('xmax')
    xmaxs = [xmax.firstChild.data for xmax in xmaxs]
    ymaxs = collection.getElementsByTagName('ymax')
    ymaxs = [ymax.firstChild.data for ymax in ymaxs]

    object_num = len(names)

    for j in range(object_num):
        if names[j] in categories_map:
            image_id = i + 1
            x1,y1,x2,y2 = int(xmins[j]),int(ymins[j]),int(xmaxs[j]),int(ymaxs[j])
            x1,y1,x2,y2 = x1 - 1,y1 - 1,x2 - 1,y2 - 1

            if x2 == width:
                x2 -= 1
            if y2 == height:
                y2 -= 1

            x,y = x1, y1
            w,h = x2 - x1 + 1, y2 - y1 + 1
            category_id = categories_map[names[j]]
            area = w * h
            annotation_info = {"id": ann_id, "image_id":image_id, "bbox":[x, y, w, h], "category_id": category_id, "area": area,"iscrowd": 0}
            annotations['annotations'].append(annotation_info)
            ann_id += 1

with  open('./annotations.json', 'w')  as f:
    json.dump(annotations, f, indent=4)

print('---整理后的标注文件---')

```

