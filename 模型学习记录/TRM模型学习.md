面试题目系列

https://zhuanlan.zhihu.com/p/352002029

https://zhuanlan.zhihu.com/p/149799951

本记录包含内容：

- 位置编码
- 多头注意力机制
- 残差和layerNomal
- 前馈神经网络
- TRM面试

1. 位置编码

   ![image-20210315231609188](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210315231609188.png)

看为黑盒子，初步细化

![image-20210315231654346](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210315231654346.png)

拆解为 编码器-解码器 类似于seq2seq

![image-20210315231750021](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210315231750021.png)

拆解为6编码6解码的方块图

**注意：6个编码器是同时训练的，不共享参数，是6个编码器都在训练。不是训练一个然后复制6个。**

6个encoder 6个decoder分别结构相同，但是参数不同

encoder和decoder结构不同

模型图：

![image-20210316102408371](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316102408371.png)

左侧是编码器，右侧是解码器

能看到编码器和解码器的结构是不同的。 解码器多了交互的多头注意力机制。

左右分开进行分析：

- 编码器

  ![image-20210316102733675](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316102733675.png)

  - 输入部分

    一个是enmedding，另一个是位置编码

    位置编码的原因：

    ​	![image-20210316102907775](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316102907775.png)

    RNN的参数是一套的，UVW是一套参数，不是好几套。每个时间步都是共享参数的。UVW是同步更新的。是有时序关系的，先处理我，在处理爱，再处理你。 

    而TRM模型是有批处理能力的，一起处理速度更快，这就忽略了句子的时序性。我在最前面，爱在中间，你在最后面。

    ![image-20210316134757245](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316134757245.png)

    偶数位置用sin，奇数位置用cos。

    ![image-20210316103819372](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316103819372.png)

     ![image-20210316134847596](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316134847596.png)

    PE公式体现了绝对位置信息，公式2推理后得到公式3提现相对位置。

    你的位置，由我、爱线性表示

  - 注意力机制

    - 基本的注意力机制

      ![image-20210316135607892](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316135607892.png)

      用图片举例

      ![image-20210316135656516](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316135656516.png)

      关键是QKV三个矩阵，公式中的d<sub>k</sub>是先不用管的。

      婴儿、左上、左下、右上、右下、v<sub>1-4</sub>分别是向量

      婴儿和左上、左下、右上、右下分别点乘，值越大说明相似度越大，越应该注意。例如0.7 0.1 0.1 0.1 再和V相乘，就得到加权和注意力。

      用文本举例

      ![image-20210316140345458](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316140345458.png)

      展开进行计算

      ![image-20210316140434873](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316140434873.png)

      爱和我、不、爱、你 分别计算，得到S<sub>1-4</sub>的结果之后再softmax归一，得到a<sub>1-4</sub>其和为1.

      之后看箭头，a<sub>i</sub>和Value<sub>i</sub>乘做和得到注意力分数

    - TRM中怎么操作

      只有词向量的情况下怎么获取QKV矩阵

      ![image-20210316140933652](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316140933652.png)

      X乘以W<sup>Q/K/V</sup>得到Q/K/V

      ![image-20210316141225558](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316141225558.png)

      实际中使用矩阵实现并行

      ![image-20210316141341569](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316141341569.png)

      W<sup>QKV</sup>是一套参数，是单头注意力。实际中会用多套参数，就是多头注意力机制。

      ![image-20210316141518007](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316141518007.png)

      ![image-20210316141709728](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316141709728.png)

      

      

  - 前馈神经网络

    接下来是残差网络和LayNorm

    ![image-20210316142212157](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316142212157.png)

    x是词向量，向上走，和位置编码对位相加，得到新的x<sub>1,2</sub> 之后进入注意力层，得到输出结果Z<sub>1,2</sub> 之后进入残差网络，将Z矩阵和输入X（位置编码之后）矩阵原封不动的拿来对位相加，再做layerNorm再输出。

    那么什么是残差？

    ![image-20210316142604500](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316142604500.png)

    两个weight可以归结为一个函数F(x),输出结果是加号上面的箭头。拿上输入X和F(x)对位相加，再输出。

    ![image-20210316143203541](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316143203541.png)

    ![image-20210316143336915](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316143336915.png)

    确保了梯度不消失，因为有1的存在。梯度不消失，网络才能提高深度。

    几乎见不到堆叠RNN的情况，最多是Bi-LSTM双层双向就很难训练了。

    

    问题**为什么不用BN batch norm?**

    很少用BN，几乎都用LN，因为BN在NLP任务重效果不好。

      BN：![image-20210316143939894](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316143939894.png)

    BN在于针对整个batch样本在同一维度做处理

    ![image-20210316144047340](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316144047340.png)

    每列代表样本，小红、小绿；每行代表特征，体重、身高。

    BN是对batch里面的所有人的每一个特征做处理，例如所有人的身高、提高。

    优点：可以解决内部协变量偏移   缓解梯度饱和问题（使用S函数的话），加快收敛

    缺点：batch_size小的话效果差   BN在RNN中效果比较差

    

    为什么用LN？为什么LN对一个样本的所有单词缩放可以起到效果。

    ![image-20210316150208959](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316150208959.png)

    按照BN的思想，将我、今进行处理，并不能代表什么信息，不能像 体重 那样作为特征来缩放。

    而LN可以，将“我爱中国共产党” 和 “今天天气真不错”每个句子的所有单词去做均值和分差。含义是认为“我爱中国共产党”语义相近。可以理解是做了加权词向量。

    

    再看前馈神经：

    ![image-20210316150558521](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316150558521.png)

    Z<sub>1,2</sub>通过feed forward两层全连接，再过一个残差和LN得到编码结果。

    

    以上就是编码过程。

- 解码器

  解码器可以分为两部分

  ![image-20210316151012818](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151012818.png)

  第一模块 掩盖的多头注意力机制

  ![image-20210316151221422](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151221422.png)

  为什么mask?

  ![image-20210316151309528](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151309528.png)

  没有mask时

  ![image-20210316151418229](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151418229.png)

  平时做题看着完全的答案，考试的时候没有答案在手边参考，做题就不好。所以需要平时练习的时候就把答案盖住。

  ![image-20210316151604846](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151604846.png)

  把you now给的信息叉掉，确保做题和考试状态的一致性。

  第二模块 交互层

  ![image-20210316151717615](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151717615.png)

  这里是典型的多头注意力机制。

  ![image-20210316151754852](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151754852.png)

  所有编码器的输出和每一个解码器去做交互。

  ![image-20210316151920666](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316151920666.png)

  记住，编码器输出的是K、V矩阵，解码器生成Q矩阵。

  

  ![image-20210316152048833](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316152048833.png)

  虚线代表KV矩阵的输出，是和每一个的解码器的交互层的Q做交互。

  

  ---

  换个角度：

  ![image-20210316181600590](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316181600590.png)


上图可以得知，feed forward是全连接层。TRM的核心就是多头注意力和masked多头注意力。

![image-20210316182017418](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316182017418.png)

x乘以矩阵得到a向量，具体的，a可以分为qkv。qkv由x分别乘以权重矩阵得到。q代表索引，k代表键,v代表数值。

**多头注意力机制=self-attention**

---

拆解来看，单头注意力过程

![image-20210316183501604](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316183501604.png)

含义是用x1要得到b1要多x3有多大注意。要做的是用x1得到的q1乘以x3得到的k3

![image-20210316183700377](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316183700377.png)

这样就能看出来，用x<sub>1</sub>生成b<sub>1,2,3,4</sub> 需要对x<sub>1,2,3,4</sub> 的注意力。a<sup>^</sup><sub><i,j></sub> 求和是1，代表xi对xj的关注率。

化简得到：

![image-20210316184025827](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316184025827.png)

最后用a<sup>^</sup><sub><i,j></sub> 乘以对应的v<sub>j</sub> 求和得到b<sub>i</sub>

![image-20210316184456041](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316184456041.png)

目标：获得b1，要对x<sub>1-4</sub> 分别给与多大的注意。

同理，扩展到多头注意力：b<sup>11</sup> 代表第一套由W<sub>q\k\v</sub> 得到的q k v参数计算出来的注意力。

![image-20210316184850659](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316184850659.png)

如果生成多组的a，比如生成a<sup>11</sup> a<sup>12</sup> 分别对应q<sup>11/12</sup> k<sup>11/12</sup> v<sup>11/12 </sup> 经过计算得到b12

![image-20210316223800243](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316223800243.png)

更换一套参数对应q<sup>12</sup> k<sup>12</sup> v<sup>12 </sup> 经过计算得到得到b<sup>12</sup>

与上一套参数对应q<sup>11</sup> k<sup>11</sup> v<sup>11 </sup> 经过计算得到得到b<sup>11</sup>

经过整合，得到b1





