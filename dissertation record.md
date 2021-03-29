# dissertation record

## idea

1.由于配置了多个摄像头，但是每个摄像头的位置不同，所以他们检测的目标，时机，和频率也不同，也许可以通过针对每个摄像头不同的工作特点，有针对性的削减所要识别的类别；或者依据车子的行进状态，开闭摄像头的检测，如前进时，后边和两侧的摄像头打开但不检测，左拐时，前方和左侧的摄像头检测，其他摄像头不检测

## 2021.3.15

https://www.bilibili.com/video/BV1v4411Y7sF?p=1

key:	the core of RCNN、Fast-RCNN、Faster-RCNN; run Faster-RCNN on colab and get the right answer.

Next two week plan: 

reproduce other work like yolo3

extensive reading related papers and record the core ideas of them

## 2021.3.29

what I do last two weeks

run yolov3 via pytorch, acquire basic idea of it ；通读此链接(https://github.com/hoya012/deep_learning_object_detection)中13-19年标红的推荐论文，整理它们的基本思想与概念；

1.yolov3

basic idea

https://blog.csdn.net/weixin_44791964/article/details/105310627

CUDA:11.2 cudnn: pytorch: torchvision:

为防止失真加入灰条

<img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210317155023450.png" alt="image-20210317155023450" style="zoom: 25%;" />

检测的基本思路

<img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210317155144183.png" alt="image-20210317155144183" style="zoom: 25%;" />

网络结构

<img src="https://img-blog.csdnimg.cn/20191020111518954.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDc5MTk2NA==,size_16,color_FFFFFF,t_70#pic_center" alt="img" style="zoom: 50%;" />

特征提取：特征提取过程是一个下采样过程(Darknet-53)：高和宽不断压缩，通道数不断增加

三个注意点：(1)Darknet-53作为主干特征提取网络 (2)特征金字塔（3）网格思想

Darknet总的来说就是下采样+残差网络的堆叠

## 历年论文的思路总结

### 1.RCNN

基本流程：

Selective search提取候选框 - 把所有候选框resize到相同大小 - 把每一个候选框送入CNN提取特征，而后进行回归与分类预测

<img src="https://mmbiz.qpic.cn/mmbiz_png/75DkJnThACk7Enjq0LvHgt8DtQeUV9nQ2E6yqjVxOicYZSmDET2Bkl4lEIlCZCMaN4YuLQDQdu3qCYVW3vuMpDA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom: 67%;" />

缺点：

1.Selective search只能运行于CPU，慢

2.每个区域都要使用一个卷积网络，而后还都要进行分类与回归，慢

3.需要resize, 失真

### 2.Overfeat

need to know: FCN、offset max-pooling

https://zhuanlan.zhihu.com/p/36596733

http://blog.csdn.net/hjimce/article/details/50187881

key points:

1. using one shared model to do classification, localization and detection--by assuming layer1 to layer 5 as the feature extraction layer, then training layer6 to output layer for localization

2. Overfeat is name of feature extractor

3. multiscale + sliding window

4. in training process, similar with Alexnet

5. the attractive work of this paper shows in testing process: Using multiscale images as input - during this process, FCN makes multiscale happens while offset max-pooling makes more predicting results happens, then you can select a result with highest probability to represent present scale. For all scales' probability, you can mean the results of them as the final result.

    <img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210328092412311.png" alt="image-20210328092412311" style="zoom:50%;" />

### 3.SPP-net

key points:

1. regardless of image size/scale
2. avoids repeatedly computing the convolutional features
3. SPP layer makes ROI have the same size

<img src="https://img-blog.csdn.net/20170618165250909?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdjFfdml2aWFu/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center" alt="img" style="zoom:50%;" />

### 4.Fast R-CNN

https://blog.csdn.net/v_JULY_v/article/details/80170182

<img src="https://img-blog.csdn.net/20180707133342715?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FpbmNoYW8zMTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="img" style="zoom:50%;" />

key point:

ROI pooling layer(similar with SPP-net) make different candidate box into same size

### 5.OHEM

![img](https://img-blog.csdn.net/20161008222116459)

![img](https://img-blog.csdn.net/20161008222120444)

key point:

1.  a improvement on Fast RCNN

question: do not know where is the improvement

### 6.YOLOV1



## 不懂的名词

池化

sliding window

SGD算法