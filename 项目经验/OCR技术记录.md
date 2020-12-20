# 任务目的：

实现扫描版本的pdf的读取和关键词读取。

---

详细的步骤：

1. 下载

   ```
   https://github.com/tesseract-ocr/tesseract
   带dev的为测试版本，稳定性得不到保障 
   ```

2. 安装

![image-20201217115143276](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201217115145.png)  

在安装的时候，选择下载赴埃及的语言数据，即可对多语言进行识别。

3. 语言包的设定

   ```
   https://github.com/tesseract-ocr/tessdata
   可以在这个网站下载，或者是选择安装步骤中选择下载。
   ```

4. 环境配置

   ```
   将tessact-OCR 文件夹路径加到path中
   ```

   ![image-20201217120000294](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201217120002.png)

![image-20201217120031013](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201217120033.png)

![image-20201217133330720](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201217133332.png)

```
tesseract --list-langs
```

查看语言包

![image-20201217133429881](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201217133432.png)

结果如上图。

5. 安装py包

   ```
   python -m pip install pyocr
   
   ```

   继续测试语言包的可用性

   ```
   from PIL import Image
   import sys
   
   import pyocr
   import pyocr.builders
   
   tools = pyocr.get_available_tools()
   if len(tools) == 0:
       print("No OCR tool found")
       sys.exit(1)
   # The tools are returned in the recommended order of usage
   tool = tools[0]
   print("Will use tool '%s'" % (tool.get_name()))
   # Ex: Will use tool 'libtesseract'
   
   langs = tool.get_available_languages()
   print("Available languages: %s" % ", ".join(langs))
   lang = langs[0]
   print("Will use lang '%s'" % (lang))
   # Ex: Will use lang 'fra'
   # Note that languages are NOT sorted in any way. Please refer
   # to the system locale settings for the default language
   # to use
   
   ```

6. 开始任务-pdf转img

   ```
   python -m pip install  fitz
   报错
   
   ```

   ![image-20201217145654173](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201217145655.png)

   去https://visualstudio.microsoft.com/zh-hans/downloads/ 下载需要的文件即可

   参考： https://blog.csdn.net/tszupup/article/details/83536673?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2.control

   ![image-20201217152601277](https://raw.githubusercontent.com/hodge-ge/imgbed/main/20201217152603.png)

   

```
上一个包是依赖这个包的：
pip install django-frontend
pip install PyMuPDF
```



