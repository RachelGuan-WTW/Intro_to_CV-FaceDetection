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
2. 数据集下载链接：https://pan.baidu.com/s/1OP_UL5VC84WjuNcB8N4Bmw 提取码：giyz 
PPT链接：https://pan.baidu.com/s/1SahW9BP64egyHgH-yRmJqA 提取码：m4nm 

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

## 数据探索
1. 图片大小不一致
2. 脸部不规整，可能包含整个人，可能有多人
3. 数据量很庞大

## 比赛程序的baseline
Baseline链接：https://pan.baidu.com/s/1crVhtB0h_UWA2lw8KRU_tA 

提取码：zij6 
