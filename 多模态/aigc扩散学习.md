<img width="734" height="400" alt="image" src="https://github.com/user-attachments/assets/3c2cb7de-be31-4f2b-b573-f40502f53902" />

<img width="742" height="391" alt="image" src="https://github.com/user-attachments/assets/02cb3c4f-06f1-4ccd-acda-3531f515f09c" />


正向过程总结：

1.一步步加噪声，图像越来越模糊

2.在实际操作当中，可以利用数学技巧，略去中间的过程，直接从X(0)->X(t)

<img width="480" height="398" alt="image" src="https://github.com/user-attachments/assets/b161cbe0-f582-4787-83f5-f2f03611d598" />


机器学习能否预测噪声

<img width="640" height="315" alt="image" src="https://github.com/user-attachments/assets/0e5bdb07-03b9-4626-b407-63881dcbe2dc" />


<img width="765" height="415" alt="image" src="https://github.com/user-attachments/assets/0ee3fe56-745c-4e53-aacf-28ca2e9812ba" />


<img width="691" height="305" alt="image" src="https://github.com/user-attachments/assets/103ee86f-bacb-4beb-aa95-b3c7ebaf9880" />


标准正态分布，采样出图片的，

<img width="626" height="353" alt="image" src="https://github.com/user-attachments/assets/b9a824c0-6589-4995-b07f-abc999eff123" />



扩散学习的基本思路：

任务分解：

每次只复原前一时刻的图片


<img width="645" height="403" alt="image" src="https://github.com/user-attachments/assets/5cdfaed3-a5a2-4d49-9155-2fca45e15249" />


<img width="983" height="495" alt="image" src="https://github.com/user-attachments/assets/bcaef137-cadb-4a34-90c9-491e8be61283" />


```python
import random
from config import *

def convert(img):
    result = [[round(s, 3) for s in ss] for ss in img]
    return result

# tensorflow手写数字图像
(x_train, y_train), (x_test, y_test) = datasets.mnist.load_data()
# 把像素值归一化成0-1
x_train = x_train / 255
result = []
count = 0
for x, y in zip(x_train, y_train):
    if y != 8:
        continue
    count += 1
    print(count)
    # 每张图片，生成100个训练样本
    for _ in range(0, 100):
        # 随机出噪声 e，这个是模型的预测目标
        rand = np.random.randn(x.shape[0], x.shape[1])
        t = random.randint(1, T)
        a1 = aerfa_m[t]**0.5
        a2 = (1 - aerfa_m[t])**0.5
        # 把x(t-1) 给计算出来，这也是模型的输入
        # 原始图像和噪声加权求和，权重参考config文件
        input_img = a1 * x + a2 * rand
        result.append(str([convert(input_img), convert(rand), t]))

with open("train_data", "w") as f:
    f.writelines("\n".join(result))
```

<img width="697" height="359" alt="image" src="https://github.com/user-attachments/assets/a3d0e1fb-75a3-49b3-81d8-9f368c37f534" />



<img width="691" height="366" alt="image" src="https://github.com/user-attachments/assets/d299d4e6-a367-4022-bc02-810a3de08109" />




<img width="680" height="275" alt="image" src="https://github.com/user-attachments/assets/8d94de44-ab85-482b-93e7-037ad7fbf6c3" />


2-D:

行/列/图像

同等效果下，transfomers比cnn占用更多参数


cnn：
局部感受也，如果要和很远的信息发生联系，模型就要做的很深


transfomers：

直接发生全局的联系


cnn：

需要配合池化，不断降低特征为度

transfomers：没有池化

trasfnoersm：需要更多的参数：

强大的算力支持

<img width="755" height="417" alt="image" src="https://github.com/user-attachments/assets/4eefccd5-9d15-4a72-a58d-ba93c94fe51a" />


                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
图像处理器，把像素0-255，变成0-1，并且归一化图像大小


<img width="583" height="326" alt="image" src="https://github.com/user-attachments/assets/cab16488-5742-4f73-924a-aaaf535c584a" />


用神经网络对图像进行分类

越靠近输入的网络，越倾向于特征提取（客观需求）

越靠近输出的网络，越倾向于进行分类的


<img width="684" height="338" alt="image" src="https://github.com/user-attachments/assets/a9b3b670-a2ce-4815-9799-aba4e0733b40" />





<img width="605" height="350" alt="image" src="https://github.com/user-attachments/assets/d6bb8d8b-5567-432b-8cbb-81dfdb83ef88" />




<img width="593" height="316" alt="image" src="https://github.com/user-attachments/assets/511b3650-3700-4207-8f91-ec29d724e1c1" />



正负样本在batchsize里的内部构造出来的

正负样本

在batchsize里的，内部构造出来的

batchsize是随机的到的

训练样本也具有随机性


<img width="570" height="359" alt="image" src="https://github.com/user-attachments/assets/ffa8c3bf-6344-4b17-aba2-34b6567ff46c" />



batchsize选多大合适的

1.如果batchszie过小，负样本太少了，训练效果不佳的

2.bactshzie不能太大，在类别🌲较小的时候

负样本不准的

<img width="568" height="327" alt="image" src="https://github.com/user-attachments/assets/b97ee7cc-1de4-479c-ab6b-91c598156ad2" />


<img width="617" height="350" alt="image" src="https://github.com/user-attachments/assets/c2088888-84b6-42fa-a3a9-c5801d09ccc8" />

分类，图文匹配的

改造的，

<img width="570" height="334" alt="image" src="https://github.com/user-attachments/assets/c7e3a5e4-203a-4e7d-9973-603fa5eb648a" />



