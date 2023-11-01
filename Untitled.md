# 开场白

爷爷奶奶，你们好。我是来自滁州学院大三的一名学生，今天由我给你们科普防电信诈骗相关的知识和相关案例。本次宣讲的主题是：预防诈骗，打击犯罪

本次宣讲将从以下四个方面讲述

我们随着科技的的进度啊，诈骗的手段也在不断地进步。特别是电信诈骗，这些年各类事件层出不穷，特别是针对我们老年人，下面我们来看看几类比较常见的案例。

# 免费养生讲座骗局

我们不能捡了芝麻丢了西瓜，因为一些小恩小惠而被诈骗分子给利用。

# 低价旅游团骗局

我们看到啊，说是带你游山玩水，结果在车上不交钱的话半山腰把你给扔下了，挺坑的是不是。

# 保健品骗局

我们爷爷奶奶要养成正确的就医看病习惯，不要相信所谓包治百病的零丹妙药，说不定还是糖果做的呢。

# 投资理财骗局

总而言之，还是不要贪小便宜，没有白白来钱的生意活。

# 冒充“公检法”

我们要铭记：公安机关不会通过电话来办案的，接到的电话都是诈骗电话。

# 温情“攻势”

所以，爷爷奶奶们也要注意，骗子来硬的不行，开始打感情牌了，用语言上的糖衣炮弹来逐渐获取信任，不论过程怎么样，他们的最终的目的不仅骗取了钱财，还伤了人心。

```python
x = tf.placeholder(tf.float32, shape=[None, IMAGESIZE])
y = tf.placeholder(tf.float32, shape=[None, NUM_CLASSES])


x_imgs = tf.reshape(x, [-1, WIDTH, HEIGHT, 1])  
W_con1 = tf.Variable(tf.random_normal([8, 8, 1, 16], stddev=0.1),
                     name='W_con1')
b_con1 = tf.Variable(tf.constant(0.1, shape=[16]), name='b_con1')
jj_con1 = tf.nn.conv2d(x_imgs, W_con1, strides=[1, 1, 1, 1],
                       padding="SAME") 

ch_con1 = tf.nn.max_pool(jh_con1, ksize=[1, 2, 2, 1], strides=[1, 2, 2, 1],
                         padding="SAME") 

W_con2 = tf.Variable(tf.random_normal([5, 5, 16, 32], stddev=0.1), name='W_con1')
b_con2 = tf.Variable(tf.constant(0.1, shape=[32]), name='b_con2')
jj_con2 = tf.nn.conv2d(ch_con1, W_con2, strides=[1, 1, 1, 1], padding="SAME")
jh_con2 = tf.nn.relu(jj_con2 + b_con2)
ch_con2 = tf.nn.max_pool(jh_con2, ksize=[1, 1, 1, 1], strides=[1, 1, 1, 1],
                         padding="SAME")

W_fc1 = tf.Variable(tf.random_normal([16 * 20 * 32, 512], stddev=0.1), name="W_fc1")
b_fc1 = tf.Variable(tf.constant(0.1, shape=[512]), name="b_fc1") 
h_fc1_flat = tf.reshape(ch_con2, [-1, 16 * 20 * 32])
h_fc1 = tf.nn.relu(tf.matmul(h_fc1_flat, W_fc1) + b_fc1)


keep_prob = tf.placeholder(tf.float32)
h_fc1_drop = tf.nn.dropout(h_fc1, keep_prob)


W_fc2 = tf.Variable(tf.random_normal([512, NUM_CLASSES], stddev=0.1), name="W_fc2")
b_fc2 = tf.Variable(tf.constant(0.1, shape=[NUM_CLASSES]), name="b_fc2")

y_con = tf.matmul(h_fc1_drop, W_fc2) + b_fc2

cross = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(labels=y, logits=y_con))

train_step = tf.train.AdamOptimizer(1e-4).minimize(cross) 

correct = tf.equal(tf.argmax(y_con, 1), tf.argmax(y, 1))
accuracy = tf.reduce_mean(tf.cast(correct, tf.float32))

init = tf.global_variables_initializer()
```

​	本学年设计旨在开发一个基于卷积神经网络的车牌字符识别系统。通过该系统，能够自动识别和提取车牌上的字符信息，为车牌识别技术的发展提供一种有效的方法。首先是数据集的收集，们收集了大量包含不同车牌样式和字符类型的数据集存放在指定的目录下，方便训练模型时的数据操作。这些数据集包括正常视角、倾斜视角、模糊图像等不同条件下的车牌图像。然后是数据预处理，我们对收集到的车牌图像进行了多种处理步骤，包括图像增强、去噪、裁剪以及对图片的初始化操作像设置宽高等。这些预处理步骤有助于提高模型对不同图像条件下的识别能力。接着是模型设计，我们采用了卷积神经网络（CNN）作为车牌字符识别系统的基本模型。通过对CNN的设计和调整，我们建立了一个多层卷积和池化结构，用于从车牌图像中提取特征，并进行字符分类。我们使用了常见的深度学习框架，如TensorFlow来实现模型的构建和训练。其次是模型训练，我们通过不断训练，是模型准确率达到95以上。最后，我们进行了测试，系统对识别出的图片进行分割和灰度处理，识别出的字符分别切割放在指定的目录下。

​	当然我们团队在实训中也遇到了一些问题，像库包的版本不一致导致Tensorflow库无法正常工作以及训练模型时性能还需要优化等。本次学年设计不仅为团队成员提供了宝贵的实践经验，还培养了解决问题和团队合作的能力。非常感谢团队同学之间的相互协作和老师提供的数据源与建议，让我们在本次实训中得到了锻炼，综合能力得到了提升。



| ![img](https://q.qlogo.cn/g?b=qq&nk=3186327486&s=40)群主chey_wzQQ:3186327486 | 软件21陈一凡  | 男   | 7年  | 潜水（0）  | 2021-10-02 | 2022-11-20 |
| ------------------------------------------------------------ | ------------- | ---- | ---- | ---------- | ---------- | ---------- |
| ![img](https://q.qlogo.cn/g?b=qq&nk=1035908787&s=40)管理员竹箫QQ:1035908787 | 群内提问即可  | 未知 | 15年 | 活跃（42） | 2019-11-02 | 2022-12-15 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=1921474394&s=40)管理员空青QQ:1921474394 | 群内提问即可  | 女   | 12年 | 潜水（0）  | 2020-10-21 | 2020-10-21 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=483654831&s=40)管理员大城小爱QQ:483654831 | 群里提问即可  | 男   | 7年  | 潜水（0）  | 2020-11-02 | 2022-12-27 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=2191591396&s=40)管理员超级火箭推动器QQ:2191591396 | 群内提问即可  | 男   | 9年  | 潜水（0）  | 2020-11-07 | 2020-11-07 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=3020616060&s=40)管理员再睡五分钟QQ:3020616060 | 群内提问即可  | 女   | 7年  | 潜水（0）  | 2021-05-11 | 2021-05-11 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=2325284922&s=40)管理员succyagQQ:2325284922 | 群内提问即可  | 未知 | 7年  | 潜水（0）  | 2021-11-03 | 2023-03-08 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=1197357869&s=40)管理员行止QQ:1197357869 |               | 男   | 13年 | 潜水（0）  | 2022-06-21 | 2023-03-08 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=244495945&s=40)我是大怪兽QQ:244495945 | 191计科刘汉文 | 男   | 15年 | 话唠（68） | 2019-11-08 | 2022-06-21 |
| ![img](https://q.qlogo.cn/g?b=qq&nk=2661092146&s=40)执末雪踏看柒月落花QQ:2661092146 |               | 未知 | 9年  | 吐槽（33） | 2020-11-01 | 2023-04-10 |

群主
chey_wz

QQ:3186327486

软件21陈一凡
男	7年	潜水（0）	2021-10-02	2022-11-20

管理员
竹箫

QQ:1035908787

群内提问即可
未知	15年	活跃（42）	2019-11-02	2022-12-15

管理员
空青

QQ:1921474394

群内提问即可
女	12年	潜水（0）	2020-10-21	2020-10-21

管理员
大城小爱

QQ:483654831

群里提问即可
男	7年	潜水（0）	2020-11-02	2022-12-27

管理员
超级火箭推动器

QQ:2191591396

群内提问即可
男	9年	潜水（0）	2020-11-07	2020-11-07

管理员
再睡五分钟

QQ:3020616060

群内提问即可
女	7年	潜水（0）	2021-05-11	2021-05-11

管理员
succyag

QQ:2325284922

群内提问即可
未知	7年	潜水（0）	2021-11-03	2023-03-08

管理员
行止

QQ:1197357869

男	13年	潜水（0）	2022-06-21	2023-03-08

我是大怪兽

QQ:244495945

191计科刘汉文
男	15年	话唠（68）	2019-11-08	2022-06-21

执末雪踏看柒月落花

QQ:2661092146

未知	9年	吐槽（33）	2020-11-01	2023-04-10

![image-20230925160847443](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251608355.png)

![](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251609975.png)

行为预警

巡查预警

两个巡查点