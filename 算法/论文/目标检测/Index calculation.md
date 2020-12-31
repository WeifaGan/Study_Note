# mAP计算指标

听说yolo-v5都来了，渣渣的我连yolo v1-v3的理解得不是特别到位，所以打算重新回顾一下yolo v1-v3 并学习v4-v5。那么就从目标检测的计算指标开始吧，我们一起来梳理一下mAP的计算。

在目标检测中，主要的计算指标是mAP(mean Average precision),意思是平均精确度。我们计算每个类别AP，求个平均就能得到mAP。

## 查准率(precision)和查全率(recall)

在理解mAP之前先来理解一下查准率(precision)和查全率(recall)。假设有这样一个混淆矩阵：

|          | 预测       | 预测       |
| -------- | ---------- | ---------- |
| 真实情况 | 正例       | 反例       |
| 正例     | TP(真正例) | FN(假反例) |
| 反例     | FP(假反例) | TN(真反例) |

查准率，顾名思义，就是描述查得有多准的问题，说的就是我们预测的正例中实际上有多少个正例。譬如有100个样本，我们预测其中有50个样子都是正例，但是在这预测的50个正例中，40个是正例，有10个是反例，那么查准率就是40/50=80%。所以有
$$
precision = \frac{TP}{TP+FP}
$$
查全率，顾名思义，就是描述查得有多全的问题，说的就是有实际中的n个正例中有多少个样本被预测出来了。譬如我们有100个样本，包括50个正例，50个反例，我们只预测对了40个正例，50个正例中只有40个被预测出来了，那么查全率就是40/50=80%。所以有
$$
recall = \frac{TP}{TP+FN}
$$
查准率和查全率是一对矛盾的度量。一般来说，查全率高的时候，查准率往往偏低，而查准率高的时候，查全率往往也偏低。譬如有100个样本，其中包括1个正例，99个反例。我们预测它全部都是正例，那当然有100%的查全率，因为所以的正例都会被预测出来了。但是查准率就很低了，因为预测的100个正例中只有1一个被预测出来了，查准率只有1%。不管你是关注查全率还是查准率，都是比较片面，一般我们都有综合考虑，于是就有了P-R曲线。

P-R曲线能直观地显示出学习器在样本总体上的查全率和查准率，如下图所示。在P-R图中，可以比较曲线“包住”的面积或者平衡点(查全率=查准率)来判断哪个学习器性能更加优秀，很明显黄线的学习器性能更好。

![图1 P-R图](https://gitee.com/weifagan/MyPic/raw/master/img/PR_.png)



## 交并比

交并比IoU衡量的是两个区域的重叠程度，是两个区域重叠面积占总面积的比例(重叠面积只算一次)，如下图所示。

![图1 P-R图](https://gitee.com/weifagan/MyPic/raw/master/img/IOU1.png)





## 目标检测如何确定TP、FP、FN and confidence/sore

因为训练验证和测试都是一个个batch的，所以这里的TP、FP、FN都是batch计算的。

**TP**:一个正确的检测，检测的IOU>=Threshold并且置信度大于阈值，即预测的边界框中分类正确并且边界框坐标达标的数量。如果一个ground truth有很多个预测框，那么选择IOU最高的预测框作为TP，其他作为FP。

**FP** : 一个错误的预测，检测的IOU<Threshod或者置信度小于阈值，即预测的边框分类不正确或者是边界框坐标不达标的数量。在预测的边界框中除了TP就是FP了。

**FN**: 没有被检测出来的ground truth。即ground truth中减去被正确预测的边框，剩下的边框数量。

**TN**：不应用，TN是所有不应被检测出来的bounding box，这种情况非常多，所以不应用该指标。

**confidence/sore**: 目标检测时候除了类别，四个坐标，还有confidence或者score，不同的模型叫法不同。不过他们的意思都是指边框包含目标概率的大小。

举个例子，一个图片检测到100个边框，其中包含有A类30个，B类30个，C类40个。如果A类的GT有4个，但是只是有2个被找到了(IOU满足条件)，那么A类的TP为2，FP为28，FN为2。不过这种情况还是比较乐观的，一个来说情况比较复杂，譬如多个预测框都对同一个GT的IOU都大于阈值或者一个预测框对于两个相邻的物体的IOU都大于阈值。



## AP计算

下图有7张图片，15个GT(绿色)，24个预测BBOX（红色框），每个检测都有置信度和标识字母(A-Y)。

![](https://gitee.com/weifagan/MyPic/raw/master/img/ap1.PNG)

这个例子中，我们认为IOU>0.3才被认为是TP，其他为FP。对于有重叠的情况，如Image2,3,4,5，选择IOU最大的作为TP，其他为FP，bounding box和其置信度如下表所示。

![](https://gitee.com/weifagan/MyPic/raw/master/img/ap2.PNG)

要计算AP，就要绘制PR曲线，就要有多对PR数据，多对数据怎么产生?

首先根据confidence进行从大到小排序，并产生累计的TP和FP，如下表。

![](https://gitee.com/weifagan/MyPic/raw/master/img/ap3.PNG)

然后计算每次累计的PR。

对于BOX R：P=TP/(TP+FP)=1/(1+0)=1，R=TP/(TP+FN)=1/15=0.066。

对于BOX Y：P=TP/(TP+FP)=1/(1+1)=0.5，R=TP/(TP+FN)=1/15=0.066

回过头来想想为什么要对confidence进行排序，其实我们是对每一行的confidence作为阈值来得到累计的PR，从而产生多对PR。

那我们把IOU进行排序是不是也可以呢，我觉得应该可以，目的不就是产生多对PR吗？但是IOU只是一个重叠程度，尽管重叠程度大，对类别预测也不一定准确。

有了多对PR数据，就可以画出PR曲线了，如下图所示。

![](https://gitee.com/weifagan/MyPic/raw/master/img/ap4.PNG)

但如上所述，我们通过11点插值和所有点插值来衡量AP。



**11点插值法**：

计算公式：
$$
AP =\frac {1}{11}\displaystyle \sum^{}_{r\in {0.1,0.2...1}}{p_{interp}(r)}
$$
其中
$$
p_{interp}(r)=max^{}_{r':r'>=r}\ p(r')
$$
根据公式，我们先是计算0:0.1:1每个$P_{interp}(r)$，可以得到下图的表格和曲线。

![](https://gitee.com/weifagan/MyPic/raw/master/img/ap8.PNG)

计算AP:

![](https://gitee.com/weifagan/MyPic/raw/master/img/ap9.PNG)



**所有点插值法**:

计算公式：
$$
AP = \displaystyle \sum^{}_{n=0}(r_{n+1}-r_{n}){p_{interp}(r_{n+1})}
$$
其中：
$$
p_{interp}(r_{n+1})=max^{}_{r':r'>=r_{n+1}}\ p(r')
$$
利用所有的PR数据，绘制的PR曲线如下图(左)，得到区间和对应的$p_{interp}(r_{n+1})$。

![](https://gitee.com/weifagan/MyPic/raw/master/img/ap10.PNG)

计算AP:

![](https://gitee.com/weifagan/MyPic/raw/master/img/AP11.PNG)



[参考链接]: https://github.com/rafaelpadilla/Object-Detection-Metrics





