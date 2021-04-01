# dissertation record

## idea

1.由于配置了多个摄像头，但是每个摄像头的位置不同，所以他们检测的目标，时机，和频率也不同，也许可以通过针对每个摄像头不同的工作特点，有针对性的削减所要识别的类别,比如处在两侧的摄像头把注意力更多的放在将要过马路的行人上，处在正前方的摄像头把注意力放在信号灯上等等；或者依据车子的行进状态，开闭摄像头的检测，如前进时，后边和两侧的摄像头打开但不检测，左拐时，方向盘传回信号，此时表明，至少后方或右侧摄像头可以不进行实时运算了，只有前方和左侧的摄像头需要检测，其他摄像头不检测，<u>总的来说就是，把算力在不同的运行姿态上进行分配，询问是否有必要或较为明显的改善</u>

2.在Faster ILOD: Incremental Learning for Object Detectors based on Faster RCNN有关KD的描述中，得知KD有一个特点：As smaller models are less expensive to evaluate, they can be deployed on less powerful hardware (such as a [mobile device](https://en.wikipedia.org/wiki/Mobile_device)) -- https://en.wikipedia.org/wiki/Knowledge_distillation



## 2021.3.15

https://www.bilibili.com/video/BV1v4411Y7sF?p=1

key:	the core of RCNN、Fast-RCNN、Faster-RCNN; run Faster-RCNN on colab and get the right answer.

Next two week plan: 

reproduce other work like yolo3

extensive reading related papers and record the core ideas of them

## 2021.4.1

1. run yolov3 via pytorch, acquire basic idea of it ；通读此链接(https://github.com/hoya012/deep_learning_object_detection)中13-19年标红的推荐论文，整理它们的基本思想与概念；

2. look through papers

3. one question about involution:

    https://mp.weixin.qq.com/s/UmumqhZW7Aqk6s8X1Aj7aA involution

    https://mp.weixin.qq.com/s/JhNwZakt-LnQI69Cuuh9bA  a set of papers

    https://github.com/d-li14/involution  github-involution

4. Swin transformer:https://mp.weixin.qq.com/s/2vhPpE9BA0JhGpSbsttiMA

     <u>作者的解读</u>：https://www.zhihu.com/question/437495132/answer/1800881612

     paper: https://arxiv.org/abs/2103.14030

     code: https://github.com/microsoft/Swin-Transformer

5. what is self-attention

     <img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210401132758814.png" alt="image-20210401132758814" style="zoom:50%;" />

     https://arxiv.org/abs/1911.03584

     两者的差别体现在数据量上

     <img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210401133549207.png" alt="image-20210401133549207" style="zoom:50%;" />

     ON THE RELATIONSHIP BETWEEN SELF-ATTENTION AND CONVOLUTIONAL LAYERS：

6. jetson nano  [NVIDIA Jetson的嵌入式系统开发人员工具包和模块](https://www.nvidia.com/en-us/autonomous-machines/embedded-systems/)

7. four articles from mentor:<img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210330153105135.png" alt="image-20210330153105135" style="zoom:50%;" />

     6.1 Faster ILOD

     我的理解：对已有model，当希望以它为基础调参后，其对原有和现有物体都有好的检测效果,但会有catastrophic forgetting问题，为解决 catastrophic forgetting,提出RPN+Multi-network adaptive distillation的方法

     ​		key point:

     ​			1.在已有模型的基础上增添新class的能力 

     ​			2.Using Faster RCNN as the fundamental network

     ​			3.framework is generic and can be applied to any object detectors using the RPN

     ​			4.Our work focuses on applying Knowledge Distillation(KD) to RPN-based object detectors to 				improve both speed and accuracy in incremental scenarios

     ​		question:

     ​			1.what is fixed external proposal generator and what is internal proposal generator



## 历年论文的思路总结

### 1.RCNN

基本流程：

Selective search to find ROI- resize boxes- put boxes into CNN to extract features - then classification and regression

<img src="https://mmbiz.qpic.cn/mmbiz_png/75DkJnThACk7Enjq0LvHgt8DtQeUV9nQ2E6yqjVxOicYZSmDET2Bkl4lEIlCZCMaN4YuLQDQdu3qCYVW3vuMpDA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1" alt="图片" style="zoom: 67%;" />

缺点：

1.Selective search algorithms runs on CPU-slow

2.too many CNN to go through

3.resize leads to the distortion

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
    
    <img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210329152537717.png" alt="image-20210329152537717" style="zoom:67%;" />
    
    

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

ROI pooling layer(similar with SPP-net) make different candidate box into same size to use, which is a improvement of R-CNN(which resize feature boxes to 217×217)

### 5.Faster R-CNN

<img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210312154132901.png" alt="image-20210312154132901" style="zoom:33%;" />

key point:

1. RPN<img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210329142034261.png" alt="image-20210329142034261" style="zoom:67%;" />
2. NMS method : boxes closer to the correct position is selected

### 6.OHEM

![img](https://img-blog.csdn.net/20161008222116459)

![img](https://img-blog.csdn.net/20161008222120444)

key point:

1.  a improvement on Fast RCNN

question: do not know where is the improvement

### 7.YOLOV1

key point:

https://zhuanlan.zhihu.com/p/94986199

https://blog.csdn.net/v_JULY_v/article/details/80170182

1. from region-based(2 stage) to region-free(1-stage)

2. <u>就好像捕鱼一样，R-CNN是先选好哪里可能出现鱼，而YOLO是直接一个大网下去，把所有的鱼都捞出来。</u>

3. accuracy is lower than RCNN, and one grid can only detect on thing

4. predicting based on  the whole picture and output class and position at one time, speeder up in this way

    <img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210329135650109.png" alt="image-20210329135650109" style="zoom:80%;" />

    bad point: 

    1.without region proposal, the accuracy of its box are not good enough

### 8.SSD

https://blog.csdn.net/qianqing13579/article/details/82106664

<img src="C:\Users\hmydw\AppData\Roaming\Typora\typora-user-images\image-20210329143559749.png" alt="image-20210329143559749" style="zoom: 50%;" />

key point: basic idea of YOLO(1 stage) + anchor similar as Faster R-CNN

### 9.R-FCN

back to questions about involution and mini-computer

## 不懂的名词

池化

sliding window

SGD算法