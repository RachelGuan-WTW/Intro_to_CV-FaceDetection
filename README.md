# Intro_to_CV-FaceDetection

## 推荐的CV学习资料
1. hands on Machine Learning with Tensorflow and Keras(百度网盘 )
2. 神经网络与深度学习 Michael Nielsen(http://neuralnetworksanddeeplearning.com/)

## Colab提供免费的GPU
1. 注册GOOGLE账号，登陆GOOGLE DRIVE 和COlab
2. 数据上传到DRIVE，然后notebooks中使用命令
3. 开启GPU，修改-开启GPU，notebooks里面使用invidia-smi可以查看是否开启成功
4. 每12个小时清理内存，保存Checkpoint

## 关于比赛的基本信息
1. 网址https://www.kesci.com/home/competition/5a6b51c4afceb51770d6a43a/content/3
2. 数据集

3. CV的三大基本任务：分类、检测识别、分割
4. 评估指标：分类问题（AUC、准确率等），检测识别（MAPE等），分割问题比较难

5. 本次比赛的类型：表情识别
- 找到人脸
- 人脸的表情分类（高兴、惊讶、悲伤、厌恶、愤怒、恐惧、中性等），所以表情识别变成了分类问题

## CV处理的两个技术：OpenCV和Tensorflow
1. OpenCV:传统方法，Hog算子，Sift算子，Haar级联合
- pip install opencv-python
2. Tensorflow：深度学习
- conda install tensorflow-gpu

## 数据图片情况
1. 图片大小不一致
2. 脸部不规整，可能包含整个人，可能有多人
3. 数据量很庞大

## 比赛程序的baseline

## 数据存储方式：
1. 5个文件夹，每个文件夹放一类图片，文件夹的名称为分类编码
2. 分为训练集和测试集，训练集包括一个文件夹存图片，另一个CSV文件存放标签和路径

## 数据探索
- 文件数（Linus bash脚本看，但是我不熟啊）
```
ls -l | grep '^-' | wc -l
```
解释：
1. ls:定位
2. -l:定位当前文件夹中所有的文件
3. |：通道
4. grep '^-':正则表达式，定位到以-结尾的文件
5. wc -l:统计当前文件夹的文件数
- 文件数（Python统计）
```python
import os
import numpy as np
lst_dir=os.listdir('./')#返回当前工作目录下的文件名，成为一个list
for path in lst_dir:
  if os.path.isdir(path):#判断路径是否合适
    lst_jpg=os.listdir(path)
    print(len(np.unique(lst_jpg)))#返回当前路径下的图片数
```
## 工具选择
1. OpenCV 读取和处理图像
2. Tensorflow Keras 建模
3. 其他三方包

## Pipeline
1. 数据预处理：截取人脸(深度学习、传统视觉)-->放缩和裁剪（放缩可能会形变，按照最短边裁剪出正方形）-->图片格式转换（RGB转化为灰度图片，）-->落入数据集
2. 模型建模：训练集和验证集划分（分数据集的随机划分地址）-->数据加载（因为图片很大，所以要随机加载一部分来训练；生成器/数据增强）-->深度学习建模（损失函数、优化方法）-->模型训练-->预测
- =============================
- 截取人脸用到的PACKAGE:[FaceRecognition](https://github.com/ageitgey/face_recognition)-用于识别图片中的人脸；
- 放缩裁剪用到的PACKAGE：cv2(transform the image from color to black-white, from black-white to gray, or from RGB to Hue Saturation and Value);
- 图片处理的标准库：[from PIL import Image](https://pillow.readthedocs.io/en/stable/reference/Image.html)可以将图片转化为数字矩阵
- 落入数据集采用TFRECORD：见下一部分
- 划分训练集和测试集使用np.random.choice随机选取，glob.glob提取指定路径下的所有文件路径


## TFRecords讲解
[tfrecord](https://blog.csdn.net/chengshuhao1991/article/details/78656724?utm_medium=distribute.pc_relevant.none-task-blog-searchFromBaidu-1.not_use_machine_learn_pai&depth_1-utm_source=distribute.pc_relevant.none-task-blog-searchFromBaidu-1.not_use_machine_learn_pai)是一种二进制文件。
TFRecords的使用分为以下几部分：
1. 生成TFRecords文件
2. 解析TFRecords文件：
3. 建模中调用TFRecords文件

在图像处理的过程中，常常会结合几个包一起使用：
- pillow:图片处理的标准包
- tqdm：for循环时候显示进度
- glob：读取某路径下面的文件

1. [下列为通用的存储数据到TFRECORD的步骤](https://blog.csdn.net/qq_16234613/article/details/91493224)
```
    writer=tf.python_io.TFRecordWriter(filename)#创建一个tfrecord容器
    for i in range(length):
        image=Image.open(image_list[i])
        if 'png' in image_list[i][-4:]:
            if image.mode=='RGB':
                r, g, b = image.split()
                image = Image.merge("RGB", (r, g, b))
            elif image.mode=='L':
                pass
            else:
                r,g, b, a = image.split()
                image = Image.merge("RGB", (r, g, b))
        image=image.resize((139,139))
        #这个地方就展开了
        image_bytes=image.tobytes()#将图像转化为数字
        #下列为定义tfrecord 的一个记录
        features={}
        features['image']=tf.train.Feature(bytes_list=tf.train.BytesList(value=[image_bytes]))
        features['label']=tf.train.Feature(int64_list=tf.train.Int64List(value=[int(label_list[i])]))
        tf_features=tf.train.Features(feature=features)
        tf_example=tf.train.Example(features=tf_features)
        tf_serialized=tf_example.SerializeToString()
        writer.write(tf_serialized)
    writer.close()
```

2. 解析TFRECORDS
- 定义TFRrecords对应的类型
- 大小调整：reshape
- 图像增强：图像旋转，变换亮度调整使用的是tf.image
- 数据矩阵的归一化
- 标签是否要onehot处理

3. 建模中调用TFRecords文件
每次随机抽取shuffle一部分作为一个batch,然后重复N遍，每一遍都是一个epoch;声明prefetch以GPU与CPU共同运行加速；生成循环iterator

## 卷积过程
1. 常用的卷积结构
- 卷积
2. 卷积层涉及到重要概念channel，padding（涉及参数SAME加padding和VALID不加padding），strides(步伐，滑动步数),kernel size, filters（不同channel使用不同kernel）, feature map（原矩阵通过kernel之后的结果）, [感受野](https://blog.csdn.net/program_developer/article/details/80958716)（输出结果在原矩阵上对应的范围）,

