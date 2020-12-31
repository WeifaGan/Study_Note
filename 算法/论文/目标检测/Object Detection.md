# FaceBoxes: A CPU Real-time Face Detector with High Accuracy

## 1. 论文要解决什么问题？

要保持高精度，还要在CPU上达到实时？还真有点难，但是Shifeng Zhang等人针对这个问题，提出了人脸检测模型FaceBoxes，表现SOTA。

## 2. FaceBoxes如何解决问题？

FaceBoxes框架如图1所示，主要包括Rapidly Digested Convolutional Layers (RDCL)和Multiple Scale Convolutional Layers (MSCL)模块，还有anchor密集策略。

![img](![](https://gitee.com/weifagan/MyPic/raw/master/img/faceboxes.PNG)

### 2.1 RDCL

RDCL的目的是为了快速下采样，让模型能够在CPU上面能达到实时。RDCL采用的方法是缩小空间大小，选择合适的卷积核大小和减少输出通道。

- 缩小空间大小：Conv1, Pool1, Conv2 and Pool2 的步长分别是4, 2, 2和 2, 空间大小快速降低了32倍。
- 合适的卷积核大小：前几层的一些核应该是比较小，以便加速，但是也应该足够大，以减轻空间大小减小而带有的信息丢失(为什么可以减少信息丢失s)。Conv1, Conv2的核大小为7x7, 5x5，所有池化层的核大小为3x3。
- 减少输出通道：使用C.ReLU减少输出通道，操作如图2(a)所示。因为C.ReLU作者统计发现底层卷积时卷积核存在负相关，也就是说假设我们本来使用10个卷积核，但是现在只需要用5个卷积核，另外5个卷积核的结果可以通过负相关得到。结果表明使用C.ReLU加速的同时也没损失精度。

![img](![](https://gitee.com/weifagan/MyPic/raw/master/img/faceboxes1.PNG)

### 2.2 MSCL

MSCL是为了得到更好地检测不同尺度的人脸。

* 深度：在MSCL模块中，随着网络的加深，便得到不同大小的特征映射(多尺度特征)。在不同大小特征映射中设置不同大小的anchor，有利于检测不同大小的人脸。
* 宽度：Inception由多个不同核大小的卷积分支组成。在这些分支中，不同的网络宽度，也有不同大小的特征映射。通过Inception，感受野也丰富了一波，有利用检测不同大小的人脸。

MSCL在多个上尺度进行回归和分类，在不同尺度下检测不同大小的人脸，能够大大提高检测的召回率。

### 2.3 Anchor密度策略

Inception3的anchor大小为32，64和128，而Conv3_2和Con4_2的anchor大小分别为256，512。anchor的平铺间隔等于anchor对应层的步长大小。例如，Con3_2的步长是64个像素点，anchor大小为256x256，这表明在输入图片上，每隔64个像素就会有一个256x256的anchor。关于anchor的平铺密度文中是这样定义的：
$$
A_{density}=A_{scale}/A_{interval}
$$
其中$A_{density}$和$A_{interval}$分别为anchor的尺度和平铺间隔。默认的平铺间隔(等于步长)默分别认为32，32，32，64和128。所以Inception的平铺密度分别为1,2,4，而Con3_2和Con4_2的平铺密度分别为4,4。

可以看出来，不同尺度的anchor之间存在平铺密度不平衡的问题，导致小尺度的人脸召回率比较低，因此，为了改善小anchor的平铺密度，作者提出了anchor密度策略。为了使anchor密集n倍，作者均匀地将$A_{number}=n^2$个anchor铺在感受野的中心附近，而不是铺在中心，如图3所示。将32x32的anchor密集4倍，64x64的anchor密集两倍，以保证不同尺度的anchor有相同的密度。

![image-20201121200018610](https://gitee.com/weifagan/MyPic/raw/master/img/faceboxes.JPG)

### 2.4 训练

**数据扩增**：

* 颜色扭曲
* 随机采样
* 尺度变换
* 水平翻转
* Face Boxes过滤：经过数据扩增的图片中，如果face boxes的中心还在图片上，则保留重叠部分，然后把高或者宽<20的过滤掉(这个操作不是很懂了，为什么把小目标过滤掉？)

**匹配策略**：训练期间，需要确定哪些anchor对应脸部的bounding box，我们首先用最佳jaccard重叠将每一张脸匹配到anchor，然后将anchor匹配到jaccard重叠大于阈值的任何一张脸。

**Loss function**: 对于分类，采用softmax loss，而回归则采用smooth L1 损失。

**Hard negative mining**：anchor 匹配后，发现很多anchor是负的，这会引入严重的正负样本不平衡。为了快速优化和稳定训练，作者对loss进行排序然后选择最小的，这样子使得负样本和正样本的比例最大3:1。

## 3 实验结果如何？

**Runtime**

![image-20201121212045119](https://gitee.com/weifagan/MyPic/raw/master/img/faceboxes1.JPG)

**Evaluation on benchmark**

在FDDB上SOTA。

## 4.对我们有什么指导意义？

* 要在CPU上达到实时，考虑一开始就对特征进行快速下采样。
* 在浅的卷积层考虑使用CReLU，可以减少计算量。
* MSCL告诉我们，检测各种不同大小的物体，考虑从深度和宽度上丰富感受野。
* anchor密度策略告诉我们考虑anchor密度以提高召回率。

# FootAndBall: Integrated player and ball detector

## 1.论文要解决什么问题？

要在高分辨率、远距离的视频中检测出足球运动员和足球。

## 2.所提算法如何解决问题？

### 2.1 检测难点

* 足球
  * size小
  * 容易模糊变形
* 运动员
  * 遮挡
  * 异常姿态
  * 相对于足球容易检测

### 2.2 FootAndBall

### 2.2.1 输入输出

**FootAndBall输入**：1920x1080

**FootAndBall主要有三个输出:**

* ball confidence map(对足球出现在某个格子的概率进行编码)
* player confidence map(对运动员出现在某个格子概率进行编码)
* player bounding boxes tensor(对运动员bounding boxes进行编码)

### 2.2.2 网络结构

FootAndBall使用了**FPN**(feature pyramid network，特征金字塔网络)思路，结合低级特征和高级特征，改善大背景下的小目标的检测。网络框架如图4所示。

<img src="https://gitee.com/weifagan/MyPic/raw/master/img/footandball.PNG" style="zoom: 80%;" />

由图4可知，较高层的卷积块上采样后与经过1x1卷积较低层的特征进行相加，输出三个head。其中Ball confidence map在比较浅的层输出，因为球比较小，也不需要太大的感受野。

由图4可知，input image宽和高分别是w,h，Ball confidence map的大小为(w/4,h/4,1)。Ball confidence map的位置$(i,j)$对应回原图$(x,y)=(\lfloor k(i-0.5),k(j-0.5) \rfloor$)，其中$k=4$。Player confidence map同理。

Player bounding boxes 编码为$(x_{bbx},y_{bbx},w_{bbx},h_{bbx})$。其中$(x_{bbx},y_{bbx})$为confidence map上对应grid ceil中点的相对位置，而$w_{bbx},h_{bbx}$为bbxes在confidence map上的宽高。

现在设置Player confidence map输出坐标为$(i,j)$，即预测出player坐标为$(i,j)$，则bounding box中心在原图上的坐标为$(x'_{bbx},y'_{bbx})=(\lfloor k(i-0.5)+x_{bbx}w \rfloor,\lfloor k(j-0.5)+y_{bbx}h \rfloor)$，其实player的预测坐标加上预测的偏移量。



### 2.2.3 损失函数

损失函数主要三个成分：ball classification loss, player classification loss and player bounding box loss。计算这三个成分都是使用二值交叉熵函数。

**ball classification loss**:

![](https://gitee.com/weifagan/MyPic/raw/master/img/footandball1.PNG)

其中，$c_{ij}^{B}$是ball confidence map上$(i,j)$位置上的值，${Pos}^{B}$是ball confidence map上GT的位置集合(GT位置的8邻域也要算上该集合)。${Neg}^B$是负样例集合，是ball confidence map上没有GT的位置集合。

**player classification loss**：

![](https://gitee.com/weifagan/MyPic/raw/master/img/footandball2.PNG)

${Pos}^{B}$没有算上GT位置的8邻域，因为feature map的size比较小。

**player bounding box loss**:

![](https://gitee.com/weifagan/MyPic/raw/master/img/footandball3.PNG)

其中$I_{(i,j) \in R^4}$是featue map位置$(i,j)$上预测的bounding box，而$ g_{(i,j)}$ 是对应的GT。

**loss**：

![](https://gitee.com/weifagan/MyPic/raw/master/img/footandball4.PNG)

因为正样例和负样例数量不平衡，所以要限制一下，正负比例最多1:3。

## 3 实验结果如何？

* 1920 x 1080 ：37 FPS

## 对我们的指导意义？

FootAndBall 主要使用全卷机网络对目标进行预测，并结合FPN的思想。

* 对于小物体检测，考虑FPN
* 可以使用全卷积在feature map上做预测。