# ICDD实验室服务器配置Python环境

### 一、更新软件源

```python
apt-get update #更新源
apt-get upgrade #更新软件
```

### 更换或内镜像源

```python
cd /etc/apt/  		#进入该目录
cp sources.list sources.list.bak 		#原来的源进行备份
vim sources.list  		#更改旧的源 内容
```

用以下内容覆盖sources.list 中的内容

写入以下内容并保存，修改后按esc键退出编辑模式，在用**:wq**来保存。

```python
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
#deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
```



```python
apt-get update 		#更新源
```



### 二、Python 3.5.2 升级到 Python 3.8

### 安装依赖包

Ubuntu 官方 apt 库中还未收录 python 3.8，这里使用 [deadsnakes](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa) PPA 库安装。

```
apt install software-properties-common
```

### 添加 deadsnakes PPA 源

```Python
add-apt-repository ppa:deadsnakes/ppa
apt-get update 		#更新源
```

### 安装 python 3.8

```
apt install python3.8

python3.8 -V		#查看Python3.8版本号
```



### 三、配置 python3.8 为系统默认 python3

### 将 python 各版本添加到 update-alternatives

~~~shell
$ which python3.8
/usr/bin/python3.8
$ update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1

$ which python3.5
/usr/bin/python3.5
$ update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 2
~~~

### 配置 python3 默认指向 python3.8

~~~shell
$ update-alternatives --config python3

There are 2 choices for the alternative python3 (providing /usr/bin/python3).

  Selection    Path                Priority   Status
------------------------------------------------------------
* 0            /usr/bin/python3.5   2         auto mode
  1            /usr/bin/python3.5   2         manual mode
  2            /usr/bin/python3.8   1         manual mode

Press <enter> to keep the current choice[*], or type selection number: 2
~~~

选择/输入2，回车。

### 测试 python 版本

~~~shell
$ python3 -V

Python 3.8.6
~~~



### 四、pip3

### 安装pip3

```shell
apt-get install python3-pip
```

### pip3升级

~~~
pip3 install --upgrade pip
~~~



### 五、安装Conda(建议Miniconda)

~~~shell
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
sh Miniconda3-latest-Linux-x86_64.sh
~~~



https://www.bilibili.com/video/BV1Jt411U7HP

```
bash Miniconda3-latest-Linux-x86_64.sh
```

设置空路径，即可安装成功



切换虚拟环境

```
conda activate base
conda activate py37
```



退出虚拟环境

~~~
conda deactivate
~~~



查看虚拟环境列表

~~~
conda env list
~~~



修改默认不进入base虚拟环境

~~~
conda config --set auto_activate_base false
~~~



### 六、创建Python 3.7 虚拟环境

~~~shell
conda create -n py37 python=3.7 anaconda
~~~



### 七、安装TensorFlow

~~~shell
pip install tensorflow-gpu==1.14.0
~~~



### 八、安装PyTorch

~~~shell
conda install pytorch torchvision cudatoolkit=10.0 -c pytorch
~~~



### 九、[pip导出当前项目所用的包list列表](https://www.cnblogs.com/yueyuecoding/p/12831799.html)

导出pip安装的所有的包：

```shell
pip freeze > piplist.txt
```

在新的环境中安装导出的包

```shell
pip install -r piplist.txt
```



### 十、[conda导出当前项目所用的包list列表](https://blog.csdn.net/qq_40263477/article/details/104290133)

批量导出

~~~
conda list -e > condalist.txt
~~~

批量安装

~~~
conda install --yes --file condalist.txt
~~~



### 十一、配置Jupyter

安装/卸载Jupyter

~~~
conda install jupyter notebook
conda uninstall jupyter
~~~



添加root启动权限

```
如果输入jupyter notebook --generate-config提示：
Running as root is not recommended. Use –allow-root to bypass.
则在后边加上 --allow-root
jupyter notebook --generate-config --allow-root
```

显示![img](https://img-blog.csdnimg.cn/20190725162858958.png)



修改配置文件

```shell
vi /root/.jupyter/jupyter_notebook_config.py
将c.NotebookApp.allow_root = False前边的  '#'去掉，在把False修改为True
```



生成登录密码

shell输入ipython，进入ipython：

~~~
from notebook.auth import passwd
passwd()
Enter password:  输入一次密码
Verify password:  再次输入密码
sha1：ae9e423f48ae:a8................................  记下密码
~~~

sha1:a3ab183828a4:641ed4898aed7cd3c8bec17f219b6b60a2a709c0

argon2:$argon2id$v=19$m=10240,t=10,p=8$vQK/sJobg2iWpSOhKXqWcg$iwWRi+ZN4rl283qgjvNVvQ



修改配置文件

~~~
vi /root/.jupyter/jupyter_notebook_config.py
c.NotebookApp.ip = '*'
c.NotebookApp.password = u'sha1:ae9e423f48ae:a8   （输入刚刚密码）
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8888    #任意空闲端口，使用默认8888也可以
~~~



Jupyter启动使用

shell输入jupyter notebook

```
jupyter notebook --ip=127.0.0.1 --port=8888
```





### 十二、使用Linux screen命令

#### screen为多重视窗管理程序

**语法**

```
screen [-AmRvx -ls -wipe][-d <作业名称>][-h <行数>][-r <作业名称>][-s <shell>][-S <作业名称>]
```

**参数说明**：

- -A 　将所有的视窗都调整为目前终端机的大小。
- -d<作业名称> 　将指定的screen作业离线。
- -h<行数> 　指定视窗的缓冲区行数。
- -m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
- -r<作业名称> 　恢复离线的screen作业。
- -R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
- -s<shell> 　指定建立新视窗时，所要执行的shell。
- -S<作业名称> 　指定screen作业的名称。
- -v 　显示版本信息。
- -x 　恢复之前离线的screen作业。
- -ls或-list 　显示目前所有的screen作业。
- -wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。



安装screen

~~~
apt-get install screen
~~~



使用screen

~~~
screen
~~~



查看screen列表

~~~
screen -list
或
screen -ls
~~~



创建名为name的后台任务

~~~
screen -S name #python helloworld.py
~~~

Cotrol + D放后台运行当前程序（关闭当前screen窗口）



终止screen后台进程

~~~
screen -S session_name -X quit
~~~















