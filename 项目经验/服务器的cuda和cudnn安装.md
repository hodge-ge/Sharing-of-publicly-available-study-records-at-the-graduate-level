# 任务： 将服务器的cuda从11.0降低到10.0

## 任务描述：

linux centos7因系统要求cuda需要降低版本。

## 解决措施：

cudn配置

1. 确定系统版本

   ~~~
   cat /etc/redhat-release
   确定centos7 系统
   ~~~

2.  查看cuda版本

    ~~~
    NVIDIA-smi
    看到cuda是10.1
    ~~~

3.  安装cuda toolkit  这就是cuda

    ~~~
    sh cuda_............_linux.run
    后面阅读协议，之后accept.
    驱动不装，示例不装。
    ~~~

4.  修改环境变量

    ~~~
    vim /etc/profile
    ~~~

    ~~~
    export PATH=/usr/local/cuda/bin:$PATH
    export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
    地址在安装完会给提示的。
    
    source /etc/profile
    ~~~

    

5.  查看安装结果

    ~~~
    nvcc --version
    ~~~

6.  安装cudnn

    ~~~
    tar -xzvf cudnn-8.0-linux-x64-v5.1.tgz
    （根据自己安装的目录适当改动）
    # 复制cudnn头文件
    sudo cp cuda/include/* /usr/local/cuda-8.0/include/
    # 复制cudnn的库
    sudo cp cuda/lib64/* /usr/local/cuda-8.0/lib64/
    # 添加可执行权限
    sudo chmod +x /usr/local/cuda-8.0/include/cudnn.h
    sudo chmod +x /usr/local/cuda-8.0/lib64/libcudnn*
    ————————————————
    版权声明：本文为CSDN博主「Jasablanca」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
    原文链接：https://blog.csdn.net/qq_34941153/article/details/105255688
    
    cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2
    ~~~

    

