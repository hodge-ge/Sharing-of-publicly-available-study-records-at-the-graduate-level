# 任务：shell集成linux命令

- 任务来源： 实验室汇总知识图谱系统。
- 任务内容： 将linux命令进行集成化，使用一条shell命令即可解决之前的复杂的目录切换，命令输入的问题。

## 问题：通过windows编辑的shell文件到linux会报错，提示空行'\r‘ ，这个问题是dos和unix字符的不兼容造成的。

## 处理方式： 在linux上 

```shell
apt-get install dos2unix
安装结束后
dos2unix XXX.sh   
这样讲就可以将文件字符进行统一。
```

注意！ 这里我也尝试过使用统一utf-8，但是并没有任何效果。

![image-20201202212800298](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201202212800.png)

![image-20201202214318144](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201202214318.png)

通过以上操作，即可将linux的命令进行集成。





后记：

文件：test_pwd.sh

内容：

```shell
#!/bin/sh  
cd /root/Neo4j/neo4j-community-3.4.1/bin
pwd
ls -l
```

运行后目录不被sh控制：

- 使用bash运行，sh文件结束后所处的路径还是root,不受到sh文件的cd切换的影响   

- ```shell
  bash test_pwd.sh
  ```

- 使用./运行，同样不受cd影响

- ````shell
  ./test_pwd.sh
  ````

```
出现Permission denied
解决：
chmod -R 777 test_pwd.sh    对sh文件进行授予最高权限
通用做法：
chmod -R 777 某一目录 对目录级联地授最高权限
```
- 使用./运行，将会受cd影响

  ![image-20201202225034937](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201202225034.png)

  

```
source ./test_pwd.sh
这里会发现 source之后，工作目录变为了sh文件最后一个cd切换到的目录
```

如何在sh中切换或者激活conda环境？

- 分析：shell调用命令是不走环境变量的，需要制定命令的路径
- 具体做法：新建change_conda.sh文件，里面包含的内容是：

```python
#!/bin/sh  
source /home/miniconda3/bin/activate py37
python --version

运行方式：
source ./change_conda.sh
```

这样就会切换到虚拟环境py37下  

遇到的问题：命名虚拟环境可以通过conda env list查看到，但是切换时提示找不到。

这是因为activate和虚拟环境不配套，一定确认好是  

```
source 创建虚拟环境A的activate绝对路径 虚拟环境A 
```

```
上面的py37是由home/miniconda3/bin下的create创建的。
而不是实验室默认的opt/conda/bin下的create创建的。
```

![image-20201203100058870](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201203100058.png)









