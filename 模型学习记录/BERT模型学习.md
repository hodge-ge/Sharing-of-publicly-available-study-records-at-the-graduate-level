# BERT解读：

目录：

https://www.bilibili.com/video/BV1Ey4y1874y

![image-20210316225810743](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210316225810743.png)

1. bert整体模型架构

   基础架构是TRM的编码器。

   ![image-20210316231128390](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210316231128390.png)

   bert-base 是12层encoder，bert-large是24层encoder

   TRM模型是6个编码器堆叠成编码端，6个decoder堆叠一起编程解码端。

   12个编码器堆叠成BERT，而不是12个TEM模型堆叠成BERT。

   

   ![image-20210317071614444](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317071614444.png)

   堆叠指的是编码器或者解码器。

   bert的编码器部分重点关注输入部分。

   对于TRM模型来说输入是 input embedding 和位置编码信息对位相加。

   ![image-20210317072240066](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317072240066.png)

   但是BERT的输入是三部分：

   ![image-20210317072138468](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317072138468.png)

   词嵌入+句子嵌入+位置嵌入（注意和TRM的positional encoding是不同的）

   bert输入部分是什么样子：

   ![image-20210317072407847](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317072407847.png![image-20210317073536594](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317073536594.png)

   

   粉色的是输入：重点看两个部分，一个是正常词汇，一个是CLS、SEP、SEP三个两种特殊符号。他们存在的意义 ：做NSP任务，next sentense prediction用来判断**两个句子**之间的关系。

   - SEP: 告诉模型 我之前是一个句子，之后是另一个家嘴
   - CLS:因为做的NSP是二分类任务，CLS输出接二分类器。CLS输出向量不能代表一个或者两个句子的语义信息。 

   黄色的是token embeddings: 对包含CLS,SEP的所有词进行embedding。

   绿色的是segment embeddings: 第一个句子全用0表示，第二个句子用1表示。

   灰色的是position embedding:  和TRM的位置编码不同，TRM是正余弦函数。BERT是随机初始化让模型学。从0-511标号index，让模型学习每个位置embedding.

2. 预训练 MLM+NSP mask language model+ next sentense prediction

   使用的语料是没标注的语料。

   ![image-20210317074026201](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317074026201.png)

   例子：我爱吃饭

   ![image-20210317074242965](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317074242965.png![image-20210317074549072](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317074549072.png)

   

   AR模型只用到单侧信息，是从左到右顺过来的。

   AE模型是遮盖mask，把用面具把吃遮盖住，利用残缺的信息学习 我爱mask饭的条件下出现我爱吃饭的概率。

   ==mask的作用：打破原有文本的信息，让模型从mask周围信息学习重建信息。==

   缺点：![image-20210317074949373](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317074949373.png)

   吃 饭独立了。 mask之间独立了，这是mask模型的缺点。

   概率

   ![image-20210317075058675](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317075058675.png)

   1000个单词，拿出来150个词汇进行处理，15个换成其他词汇干扰，15个词汇不动，120个词汇换成mask。

   ![image-20210317075445189](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317075445189.png)

   mask_indices 是15%的词汇

   接下来看NSP任务：

   ![image-20210317075646118](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317075646118.png)

   正样本：一个主题两个连续段落，顺序没颠倒

   负样本：不同主题不连续段落。

   主题预测和连贯性搞成一个任务了。主题预测很简单。albert抛弃掉了主题预测。

3. 微调bert

   ![image-20210317080253676](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317080253676.png)

   句子对分类、单句子分类、问答系统、序列标注。

   序列标注：其实就是对所有token输出，做softmax，看属于哪个标签

   

   如何提升bert在下游任务的表现：

   一般做法：

   ![image-20210317080634280](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317080634280.png)

   

   分解为四个步骤：

   ![image-20210317091112235](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317091112235.png)

   第一步：我们是不做的，直接拿谷歌的来用

   第二步：是领域的自适应，领域迁移。

   ​	![image-20210317095642880](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317095642880.png)

   ​	![image-20210317095918742](D:\github_data\Sharing-of-publicly-available-study-records-at-the-graduate-level\模型学习记录\BERT模型学习.assets\image-20210317095918742.png)

   

   第三步：聚焦文本分类

   第四步：具体任务

   

   

   

4. 代码解析

