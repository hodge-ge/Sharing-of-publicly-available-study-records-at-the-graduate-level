面试题目系列

https://zhuanlan.zhihu.com/p/352002029

https://zhuanlan.zhihu.com/p/149799951

https://www.bilibili.com/video/BV1Di4y1c7Zm

本记录包含内容：

- 位置编码
- 多头注意力机制
- 残差和layerNomal
- 前馈神经网络
- TRM面试

1. 位置编码

   ![image-20210315231609188](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183029.png)

看为黑盒子，初步细化

![image-20210315231654346](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183050.png)

拆解为 编码器-解码器 类似于seq2seq

![image-20210315231750021](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183055.png)

拆解为6编码6解码的方块图

**注意：6个编码器是同时训练的，不共享参数，是6个编码器都在训练。不是训练一个然后复制6个。**

6个encoder 6个decoder分别结构相同，但是参数不同

encoder和decoder结构不同

模型图：

![image-20210316102408371](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183101.png)

左侧是编码器，右侧是解码器

能看到编码器和解码器的结构是不同的。 解码器多了交互的多头注意力机制。

左右分开进行分析：

- 编码器

  ![image-20210316102733675](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183110.png)

  - 输入部分

    一个是enmedding，另一个是位置编码

    位置编码的原因：

    ​	![image-20210316102907775](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183115.png)

    RNN的参数是一套的，UVW是一套参数，不是好几套。每个时间步都是共享参数的。UVW是同步更新的。是有时序关系的，先处理我，在处理爱，再处理你。 

    而TRM模型是有批处理能力的，一起处理速度更快，这就忽略了句子的时序性。我在最前面，爱在中间，你在最后面。

    ![image-20210316134757245](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183123.png)

    偶数位置用sin，奇数位置用cos。

    ![image-20210316103819372](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183132.png)

     ![image-20210316134847596](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183128.png)

    PE公式体现了绝对位置信息，公式2推理后得到公式3提现相对位置。

    你的位置，由我、爱线性表示

  - 注意力机制

    - 基本的注意力机制

      ![image-20210316135607892](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183143.png)

      用图片举例

      ![image-20210316135656516](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183147.png)

      关键是QKV三个矩阵，公式中的d<sub>k</sub>是先不用管的。

      婴儿、左上、左下、右上、右下、v<sub>1-4</sub>分别是向量

      婴儿和左上、左下、右上、右下分别点乘，值越大说明相似度越大，越应该注意。例如0.7 0.1 0.1 0.1 再和V相乘，就得到加权和注意力。

      用文本举例

      <img src="https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183209.png" alt="image-20210316140345458" style="zoom:80%;" />

      展开进行计算

      ![image-20210316140434873](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183156.png)

      爱和我、不、爱、你 分别计算，得到S<sub>1-4</sub>的结果之后再softmax归一，得到a<sub>1-4</sub>其和为1.

      之后看箭头，a<sub>i</sub>和Value<sub>i</sub>乘做和得到注意力分数

    - ==从本质上理解，Attention是从大量信息中有筛选出少量重要信息，并聚焦到这些重要信息上，忽略大多不重要的信息。权重越大越聚焦于其对应的Value值上，即权重代表了信息的重要性，而Value是其对应的信息。==

    - TRM中怎么操作

      只有词向量的情况下怎么获取QKV矩阵

      ![image-20210316140933652](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183223.png)

      X乘以W<sup>Q/K/V</sup>得到Q/K/V

      ![image-20210316141225558](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183228.png)

      实际中使用矩阵实现并行

      ![image-20210316141341569](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183233.png)

      W<sup>QKV</sup>是一套参数，是单头注意力。实际中会用多套参数，就是多头注意力机制。

      ![image-20210316141518007](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183238.png)

      ![image-20210316141709728](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183243.png)

      

      

  - 前馈神经网络

    接下来是残差网络和LayNorm

    <img src="https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183259.png" alt="image-20210316142212157" style="zoom:80%;" />

    x是词向量，向上走，和位置编码对位相加，得到新的x<sub>1,2</sub> 之后进入注意力层，得到输出结果Z<sub>1,2</sub> 之后进入残差网络，将Z矩阵和输入X（位置编码之后）矩阵原封不动的拿来对位相加，再做layerNorm再输出。

    那么什么是残差？

    ![image-20210316142604500](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TFM模型学习.assets\image-20210316142604500.png)

    两个weight可以归结为一个函数F(x),输出结果是加号上面的箭头。拿上输入X和F(x)对位相加，再输出。

    ![image-20210316143203541](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183341.png)

    ![image-20210316143336915](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183325.png)

    确保了梯度不消失，因为有1的存在。梯度不消失，网络才能提高深度。

    几乎见不到堆叠RNN的情况，最多是Bi-LSTM双层双向就很难训练了。

    

    问题**为什么不用BN batch norm?**

    很少用BN，几乎都用LN，因为BN在NLP任务重效果不好。

      BN：![image-20210316143939894](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183315.png)

    BN在于针对整个batch样本在同一维度做处理

    ![image-20210316144047340](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183350.png)

    每列代表样本，小红、小绿；每行代表特征，体重、身高。

    BN是对batch里面的所有人的每一个特征做处理，例如所有人的身高、提高。

    优点：可以解决内部协变量偏移   缓解梯度饱和问题（使用S函数的话），加快收敛

    缺点：batch_size小的话效果差   BN在RNN中效果比较差

    

    为什么用LN？为什么LN对一个样本的所有单词缩放可以起到效果。

    ![image-20210316150208959](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183356.png)

    按照BN的思想，将我、今进行处理，并不能代表什么信息，不能像 体重 那样作为特征来缩放。

    而LN可以，将“我爱中国共产党” 和 “今天天气真不错”每个句子的所有单词去做均值和分差。含义是认为“我爱中国共产党”语义相近。可以理解是做了加权词向量。

    

    再看前馈神经：

    ![image-20210316150558521](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183400.png)

    Z<sub>1,2</sub>通过feed forward两层全连接，再过一个残差和LN得到编码结果。

    

    以上就是编码过程。

- 解码器

  解码器可以分为两部分

  ![image-20210316151012818](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183411.png)

  第一模块 掩盖的多头注意力机制

  ![image-20210316151221422](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183419.png)

  为什么mask?

  ![image-20210316151309528](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183454.png)

  没有mask时

  <img src="https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183435.png" alt="image-20210316151418229" style="zoom:67%;" />

  平时做题看着完全的答案，考试的时候没有答案在手边参考，做题就不好。所以需要平时练习的时候就把答案盖住。

  <img src="https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183445.png" alt="image-20210316151604846" style="zoom:50%;" />

  把you now给的信息叉掉，确保做题和考试状态的一致性。

  第二模块 交互层

  ![image-20210316151717615](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183505.png)

  这里是典型的多头注意力机制。

  ![image-20210316151754852](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183547.png)

  所有编码器的输出和每一个解码器去做交互。

  ![image-20210316151920666](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183556.png)

  记住，编码器输出的是K、V矩阵，解码器生成Q矩阵。

  

  ![image-20210316152048833](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183558.png)

  虚线代表KV矩阵的输出，是和每一个的解码器的交互层的Q做交互。

  

  ---

  换个角度：

  ![image-20210316181600590](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183609.png)


上图可以得知，feed forward是全连接层。TRM的核心就是多头注意力和masked多头注意力。

![image-20210316182017418](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\TRM模型学习.assets\image-20210316182017418.png)

x乘以矩阵得到a向量，具体的，a可以分为qkv。qkv由x分别乘以权重矩阵得到。q代表索引，k代表键,v代表数值。

**多头注意力机制=self-attention**

---

拆解来看，单头注意力过程

![image-20210316183501604](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183620.png)

含义是用x1要得到b1要多x3有多大注意。要做的是用x1得到的q1乘以x3得到的k3

![image-20210316183700377](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183626.png)

这样就能看出来，用x<sub>1</sub>生成b<sub>1,2,3,4</sub> 需要对x<sub>1,2,3,4</sub> 的注意力。a<sup>^</sup><sub><i,j></sub> 求和是1，代表xi对xj的关注率。

化简得到：

![image-20210316184025827](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183633.png)

最后用a<sup>^</sup><sub><i,j></sub> 乘以对应的v<sub>j</sub> 求和得到b<sub>i</sub>

![image-20210316184456041](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183639.png)

目标：获得b1，要对x<sub>1-4</sub> 分别给与多大的注意。

同理，扩展到多头注意力：b<sup>11</sup> 代表第一套由W<sub>q\k\v</sub> 得到的q k v参数计算出来的注意力。

![image-20210316184850659](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183646.png)

如果生成多组的a，比如生成a<sup>11</sup> a<sup>12</sup> 分别对应q<sup>11/12</sup> k<sup>11/12</sup> v<sup>11/12 </sup> 经过计算得到b12

![image-20210316223800243](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183653.png)

更换一套参数对应q<sup>12</sup> k<sup>12</sup> v<sup>12 </sup> 经过计算得到得到b<sup>12</sup>

与上一套参数对应q<sup>11</sup> k<sup>11</sup> v<sup>11 </sup> 经过计算得到得到b<sup>11</sup>

经过整合，得到b1



---

换个角度讲

https://www.bilibili.com/video/BV1P4411F77q/?spm_id_from=333.788.recommend_more_video.5

TRM理解：

​	TEM是2017年谷歌大佬提出的。Bert是TRM模型衍生出来的预训练模型。

​	应用：  培养作家

- 上游任务  学习识字

  训练预训练模型

- 下游任务   学习写作

  NLP实际任务，问答、NER

![image-20210317102227260](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183659.png)

TRM和LSTM区别：

- LSTM的训练是迭代的，一个接着一个来，当前这个字过万LSTM，下个字才能过。像是for循环。
- TRM训练是并行的，所有字同时训练，用位置嵌入理解（positional encoding）文字时序。

TRM模型：

![img](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183708.jpg)

==关键的点：解码器的输出又落下来作为输入了== 

![img](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183713.jpg)![img](https://github.com/aespresso/a_journey_into_math_of_ml/raw/e081b67d51a8dc74daa55bb0de35de86acdaa536/03_transformer_tutorial_1st_part/imgs/encoder.jpg)

- 编码器

  1. 位置编码

     - 输入的形状：X [句子个数，句子长度]

     - 之后进入embedding模块，得到嵌入。形状[句子个数，句子长度，嵌入维度]

       数据来源是 字向量表

       ![image-20210317135258679](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183737.png)vocab_size是整个词典里面的字

       embedding_size是用多少个数字表示每个字 常设为300

       例如 学 一行数字就表示了学的字向量

     - positional encoding:

       位置嵌入的维度和字嵌入的维度维度是一致的。位置嵌入向量和字嵌入向量元素相加即可。

       用sin cos分别计算偶数、奇数位置的位置信息:

       ![image-20210318183903849](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183903.png)

       可以发现，周期变化随着序号变大，会越来越慢。

       ![image-20210317144418899](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183911.png)

       这就可以让模型依据纹理的变化学习到位置信息。

  2. 多头注意力机制

     ![img](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183915.jpg)

     QK<sup>T</sup> 点积，计算出两个向量的相似度，也就是

     ![image-20210317145839324](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183924.png)

     如果两个词的意思相似，那么点积就大。

     用QK<sup>T</sup> 得到的是注意力方形矩阵，每行代表的是当前的字和这句话所有字的关系。主对角线就是这个字和本身的注意度。为了让当前词和本句的词的关联度和为1，形成概率分布，需要进行softmax归一化。需要先除以k的维度，也就是head_size。

     之后得到已经归一化的注意力机制方形矩阵，需要用V加权。V是字向量经过线性变换。目前还没动过。

     ![image-20210317163902340](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183938.png)

     左边的行代表当前词和本句所有的词之间的相关性，右边矩阵V的每行是当前这个字的数学表达。

     用第一个字和本句所有字的相关性乘以本句所有字的字向量的第一个维度，把这个字的和句子的相关性进行了计算。计算完毕之后V是没有改变的。

  3. 残差网络和LN

     残差连接的目的是反向求导的时候，能直接到X而不必要过注意力机制块。

     ![image-20210317170027021](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318183947.png)

  4. 前馈网络

     就是线性层

  整体结构：

  ![image-20210318172556906](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318184003.png)

  ![image-20210318174202028](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318184019.png)

  QKV都是输入X经过线性变换得到的矩阵，形状一样。Q和K的转置相乘含义： Q每行是每个词的字向量表示，K的转置的每列是每个字的字向量表示，通过点积也就是矩阵乘法，得到的是每个字之间的相关度。 因为QKV是由经过加上编码矩阵和X的和求线性运算得到的，因此同时也考虑到了位置信息。如果单把位置矩阵拿出来（不加 X）将会得到主对角线最大值的方阵，也就是位置矩阵。

  经过计算得到了每个字之间的相关度，得到注意力方阵。之后将注意力方阵乘以V得到和V形状一样的隐藏矩阵。相乘的含义是：注意力矩阵是当前词和当前词的所在句所有词的相关度，乘以每个词的词向量表示的第一维度，将信息融入到词向量表示中。

- 解码器

第二部分:

![image-20210317215719564](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318184028.png)

1. 位置编码 sin cos线性变换函数，两个字越近向关联度越大。这也符合基础假设：越近越相关。

   由注意力矩阵的得来来说明位置编码能实现序列信息

   注意力矩阵每个元素是说当前字和所在句的所有词的相关度。位置编码和注意力矩阵类似，也是说的当前字和所在句的所有词的位置相关性。  都是主对角线最高，自己和自己最相关。

2.  Bidirectional Encoder Representations from Transformers, 如果翻译过来也就是**双向transformer编码表达** 

   ![img](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20210318184034.png)

   每层的TRM能拿到所有输入的信息，这就是双向。

   在BERT中两种预训练的方式来建立语言模型：

   1. Masked LM

      就是随机遮盖或者替换，

   2. NSP next sentence prediction

      













