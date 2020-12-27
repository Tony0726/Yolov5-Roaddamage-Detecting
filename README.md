# Yolov5实现道路裂缝检测
[参考代码链接](https://github.com/Tony0726/yolov5)

[Github代码链接](https://github.com/Tony0726/Yolov5-Roaddamage-Detecting)

[百度网盘链接](https://pan.baidu.com/s/1DEu0TYcdowtt6k_A1wOxTQ)

密码：2mzl

基于Pytorch的Yolov5道路裂缝检测程序运行说明。大家可以结合我的说明和原文说明使用，有问题欢迎询问。

@[toc]
## 环境要求

Python 3.8或之后的版本

还要安装`requirements.txt`文件中所有依赖包，包括1.7及以上版本的`torch`

```python
$ pip install -r requirements.txt
```

## 测试

运行前先将图片或视频文件放在和detect.py同一目录下，然后运行下面语句：

```python
$ python detect.py --source 20200827153531.mp4  # video
							file.jpg  # image 
```

因为我将训练好的模型已经放入`./runs/train/exp_1000/weights/`路径下了，如果自己训练了模型后，记得修改为自己的模型路径。

原图标记：![](https://img-blog.csdnimg.cn/20201218114715641.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NzUxMzc5,size_16,color_FFFFFF,t_70)测试标记：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201218114749495.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzI2NzUxMzc5,size_16,color_FFFFFF,t_70)
因为不能放视频，所以上传到了百度云盘，大家可以自取（包含原视频和结果）。

```
https://pan.baidu.com/s/1DEu0TYcdowtt6k_A1wOxTQ
密码：2mzl
```

## 训练自己的数据集

### 1.创建dataset.yaml文件

 文件要满足以下格式（如下图）：

1. 下载地址（没有的不用管它）
2. 训练图片路径
3. 验证图片路径
4. 类的个数
5. 类名

```
# download command/URL (optional)
download: https://github.com/ultralytics/yolov5/releases/download/v1.0/coco128.zip

# train and val data as 1) directory: path/images/, 2) file: path/images.txt, or 3) list: [path1/images/, path2/images/]
train: ../coco128/images/train2017/
val: ../coco128/images/train2017/

# number of classes
nc: 80

# class names
names: ['person', 'bicycle', 'car', 'motorcycle', 'airplane', 'bus', 'train', 'truck', 'boat', 'traffic light',
        'fire hydrant', 'stop sign', 'parking meter', 'bench', 'bird', 'cat', 'dog', 'horse', 'sheep', 'cow',
        'elephant', 'bear', 'zebra', 'giraffe', 'backpack', 'umbrella', 'handbag', 'tie', 'suitcase', 'frisbee',
        'skis', 'snowboard', 'sports ball', 'kite', 'baseball bat', 'baseball glove', 'skateboard', 'surfboard',
        'tennis racket', 'bottle', 'wine glass', 'cup', 'fork', 'knife', 'spoon', 'bowl', 'banana', 'apple',
        'sandwich', 'orange', 'broccoli', 'carrot', 'hot dog', 'pizza', 'donut', 'cake', 'chair', 'couch',
        'potted plant', 'bed', 'dining table', 'toilet', 'tv', 'laptop', 'mouse', 'remote', 'keyboard', 
        'cell phone', 'microwave', 'oven', 'toaster', 'sink', 'refrigerator', 'book', 'clock', 'vase', 'scissors', 
        'teddy bear', 'hair drier', 'toothbrush']
```

### 2.创建标签文件

 文件要满足以下格式（如下图）：

1. 一张图片一个txt文件
2. 一行一个目标
3. 每行都是`class x_center y_center width height`的格式，也就是类对应的序号，目标的x轴中心点，目标的y轴中心点，还有宽和高，注意都不超过1，都是像素点除以宽或高。（一般的正规数据集都自带这种格式的标签，如果没有这种格式的，需要自己编写程序转换，如果没有标签只有图片，需要自行下载标记软件，然后标记图片）
4. 序号从0开始

```
0 0.9583333333333334 0.9408333333333334 0.07333333333333333 0.08833333333333333
2 0.7958333333333334 0.8391666666666667 0.4083333333333334 0.04833333333333334
0 0.4083333333333334 0.8508333333333334 0.17666666666666667 0.12166666666666667
```

### 3.组织文件路径

注意第1步`创建dataset.yaml文件`中的文件路径，自己填什么路径就把文件放在什么路径。

### 4.选择模型

推荐选择YOLOv5s，小还快。

### 5.开始训练

如果用的是Pycharm就右键`train.py`文件`open in terminal`,输入以下代码，如果不是Pycharm，可以再cmd中，先调到train.py路径下，再运行下面语句开始训练。（`--`之后代表参数，img就是图片要缩放的大小，最好是和原图一样大小，epochs是要迭代的次数，data就是第一步创建的文件，weight也就是训练好的权重）

```
# Train YOLOv5s on COCO128 for 5 epochs
python train.py --img 640 --batch 16 --epochs 5 --data coco128.yaml --weights yolov5s.pt
CO128 for 5 epochs
python train.py --img 640 --batch 16 --epochs 5 --data coco128.yaml --weights yolov5s.pt
```
