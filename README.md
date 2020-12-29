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



