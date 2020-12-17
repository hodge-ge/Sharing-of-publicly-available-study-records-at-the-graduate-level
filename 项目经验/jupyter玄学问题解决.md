

1. 在克隆环境win10中遇到的问题： 新建文件，就爆出kera内核错误的问题

   解决方式：

   ```
   激活环境 conda activate win10env
   首先生成配置文件jupyter_notebook_config.py
   之后找到这个配置文件：c.NotebookApp.notebook_dir，定位到下面这行写上文件存储路径。
   jupyter notebook --generate-config
   查看安装的内核和位置jupyter kernelspec list
   按照上一步给出的位置找到kernel.json文件打开。
   将当前所在的win10env虚拟环境写入内核：python -m ipykernel install --user
   ```

   2.OCR的安装

   ```
   https://huaweicloud.blog.csdn.net/article/details/92786494
   ```

   pip install pyocr

   

    --list-langs

2. 语言包配置

   ```
   https://huaweicloud.blog.csdn.net/article/details/92786494
   ```

   