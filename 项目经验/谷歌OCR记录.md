谷歌OCR的官方代码：https://gitlab.gnome.org/World/OpenPaperwork/pyocr

https://gitlab.gnome.org/World/OpenPaperwork/pyocr

语言包：

https://github.com/tesseract-ocr/tessdata/tree/3.04.00

python 时间：https://blog.csdn.net/weixin_30672295/article/details/99636710?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1.control



图片切割 https://blog.csdn.net/qq_43145035/article/details/83270501?utm_medium=distribute.pc_relevant.none-task-blog-OPENSEARCH-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-2.control

https://blog.csdn.net/RogerFedereYY/article/details/110632227?utm_medium=distribute.pc_relevant.none-task-blog-title-2&spm=1001.2101.3001.4242

多线程：https://www.cnblogs.com/fnng/p/3670789.html

 Python 获取list中指定元素的索引https://blog.csdn.net/orangefly0214/article/details/81387077

队列：https://www.cnblogs.com/yhleng/p/9493457.html

https://www.cnblogs.com/yhleng/p/9493457.html



python列表删除元素 http://c.biancheng.net/view/2209.html

OCR全平台安装  https://huaweicloud.blog.csdn.net/article/details/92786494

https://blog.csdn.net/weixin_43321865/article/details/86798930



csv https://www.cnblogs.com/qican/p/11122206.html



## 写文件遇到的问题

在cmd中使用 pip3 install docx 安装模块 docx 后，发现不能正常使用，并报错 moduleNotFoundError:No module named 'exceptions'

1、卸载原来安装的docx

```bash
pip uninstall docx
```

2、安装 **python-docx** 模块即可

```bash
pip install python-docx
```

## 字典排序的问题

遇到了多线程并发的操作，并发过后需要对数据进行排序。

```
'jfjjyimg0.png', 'jfjjyimg1.png', 'jfjjyimg2.png', 'jfjjyimg3.png', 'jfjjyimg4.png', 'jfjjyimg5.png', 'jfjjyimg6.png', 'jfjjyimg7.png', 'jfjjyimg8.png', 'jfjjyimg9.png', 'jfjjyimg10.png', 'jfjjyimg11.png', 'jfjjyimg12.png', 'jfjjyimg13.png', 'jfjjyimg14.png', 'jfjjyimg15.png', 'jfjjyimg16.png', 'jfjjyimg17.png', 'jfjjyimg18.png', 'jfjjyimg19.png', 'jfjjyimg20.png', 'jfjjyimg21.png', 'jfjjyimg22.png', 'jfjjyimg23.png', 'jfjjyimg24.png', 'jfjjyimg25.png', 'jfjjyimg26.png', 'jfjjyimg27.png', 'jfjjyimg28.png', 'jfjjyimg29.png', 'jfjjyimg30.png', 'jfjjyimg31.png', 'jfjjyimg32.png', 'jfjjyimg33.png', 'jfjjyimg34.png', 'jfjjyimg35.png', 'jfjjyimg36.png', 'jfjjyimg37.png', 'jfjjyimg38.png', 'jfjjyimg39.png', 'jfjjyimg40.png', 'jfjjyimg41.png', 'jfjjyimg42.png', 'jfjjyimg43.png', 'jfjjyimg44.png', 'jfjjyimg45.png', 'jfjjyimg46.png', 'jfjjyimg47.png', 'jfjjyimg48.png', 'jfjjyimg49.png', 'jfjjyimg50.png', 'jfjjyimg51.png', 'jfjjyimg52.png', 'jfjjyimg53.png', 'jfjjyimg54.png', 'jfjjyimg55.png', 'jfjjyimg56.png', 'jfjjyimg57.png', 'jfjjyimg58.png', 'jfjjyimg59.png', 'jfjjyimg60.png', 'jfjjyimg61.png', 'jfjjyimg62.png', 'jfjjyimg63.png', 'jfjjyimg64.png', 'jfjjyimg65.png', 'jfjjyimg66.png', 'jfjjyimg67.png', 'jfjjyimg68.png', 'jfjjyimg69.png', 'jfjjyimg70.png', 'jfjjyimg71.png', 'jfjjyimg72.png', 'jfjjyimg73.png', 'jfjjyimg74.png', 'jfjjyimg75.png', 'jfjjyimg76.png', 'jfjjyimg77.png', 'jfjjyimg78.png', 'jfjjyimg79.png', 'jfjjyimg80.png', 'jfjjyimg81.png', 'jfjjyimg82.png', 'jfjjyimg83.png', 'jfjjyimg84.png', 'jfjjyimg85.png', 'jfjjyimg86.png', 'jfjjyimg87.png', 'jfjjyimg88.png', 'jfjjyimg89.png', 'jfjjyimg90.png', 'jfjjyimg91.png', 'jfjjyimg92.png', 'jfjjyimg93.png', 'jfjjyimg94.png', 'jfjjyimg95.png', 'jfjjyimg96.png', 'jfjjyimg97.png', 'jfjjyimg98.png', 'jfjjyimg99.png', 'jfjjyimg100.png', 'jfjjyimg101.png', 'jfjjyimg102.png', 'jfjjyimg103.png', 'jfjjyimg104.png', 'jfjjyimg105.png', 'jfjjyimg106.png', 'jfjjyimg107.png', 'jfjjyimg108.png', 'jfjjyimg109.png', 'jfjjyimg110.png', 'jfjjyimg111.png', 'jfjjyimg112.png', 'jfjjyimg113.png', 'jfjjyimg114.png', 'jfjjyimg115.png', 'jfjjyimg116.png', 'jfjjyimg117.png', 'jfjjyimg118.png', 'jfjjyimg119.png', 'jfjjyimg120.png', 'jfjjyimg121.png', 'jfjjyimg122.png', 'jfjjyimg123.png', 'jfjjyimg124.png', 'jfjjyimg125.png', 'jfjjyimg126.png', 'jfjjyimg127.png', 'jfjjyimg128.png', 'jfjjyimg129.png', 
```

这类的数据是如果按照普通的sort排序将会遭遇到2比 101大的情况，因为是按照字符串字典的规则进行排序的。

如今，我们的需求是另101大于2.

```python
from collections import OrderedDict
dict = OrderedDict([
('user1', [1, 1]), 
('user5', [16]),
('user3', [1, 1]), 
('user10', [1]),
('user6', [3])])
```

```python
#直接使用常规调用排序代码
dict = OrderedDict(sorted(dict.items()，key=lambda x:x[0]) )
#结果
OrderedDict([
('user1', [1, 1]), 
('user10', [1]), 
('user3', [1, 1]), 
('user5', [16]), 
('user6', [3])])
```

可以看到user10排到了user3的前面，是谷歌OCR的官方代码：https://gitlab.gnome.org/World/OpenPaperwork/pyocr

```
dict = OrderedDict(sorted(dict.items(),key=lambda x:int(x[0][4:])))
#[4:]这里说的是从4开始后面的截取为Int，如果有文件后缀 例如“.jpg”那么久修改代码为[4:-4]即可
#从user之后的第四位开始取值，然后转换为int型
#结果
OrderedDict([
('user1', [1, 1]), 
('user3', [1, 1]), 
('user5', [16]), 
('user6', [3]), 
('user10', [1])])

```

