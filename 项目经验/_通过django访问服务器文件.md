## 任务描述：

**使用django访问服务器的文件需求：**

在项目后期完善功能时，需要对文件进行定位。 需要使用前端的html超链接访问到文件，而不是后台处理结果通过html返回前端展示。

### 首先聚焦原理：

---

```
urlpatterns = [
        url(r'^file/(?P<path>.*)$',serve,{"document_root":"D:/project/"})
]
document_root对应的值"D:/project/" 就是前端可访问的文件路径
对应关系为：
file之后内容匹配 Project之后的内容
<url>/file/sample.png <==> D:/project/sample.png 
http://211.82.97.238:7388/txt/M1坦克.txt   <====> /root/wqzb/search_resource/txt/M1坦克.txt 

```

### 下面聚焦应用模板

解决方式：

1. 修改url文件

```
from django.conf.urls import url 
from django.views.static import serve
from django.urls import path, re_path

在urlpatterns 映射列表中添加：
re_path(r'^txt/(?P<path>.*)$', serve, {'document_root': '/root/wqzb/search_resource/txt/'}),
re_path(r'^img/(?P<path>.*)$', serve, {'document_root': '/root/wqzb/search_resource/img/'}),
re_path(r'^video/(?P<path>.*)$', serve, {'document_root': '/root/wqzb/search_resource/video/'}),
```



1. 修改view文件内的后台函数返回值

```
打开文件的时候加上路径
        with open("/root/wqzb/search_resource/txt/"+onefile, 'r', encoding='GBK') as f:
            one_pian = f.readlines()
            one_pian = ' '.join(one_pian)
            pianzhang.append(one_pian)
超链接的内容应该是“ http://211.82.97.238:7388/txt/坦克.txt” 其中txt可以从第一部中的txt\img\video字段中挑选，分别到不同的路径下。
def geturl(path, content, usertype, alltype=False):
    '''
    给定path和关键词，获取包含关键词的文件名字
    '''
    file_url = []
    dirs = os.listdir(path)  # 获取到所有文件的名字列表
    #     print('超链接函数获取的路径',path,'路径下所有内容',dirs)
    lasturl = path.split('/')[-2]  # txt/img....
    #     print('从路径解析获取得到文件类型',lasturl)
    # http://211.82.97.238:7388/txt/   这是文件的存储位置
    for filename in dirs:
        if content in filename and (usertype == filename.split('.')[-1] or alltype):
            file_url.append('http://211.82.97.238:7388/' + lasturl + '/' + filename)
            # http://211.82.97.238:7388/   txt/  moumou.txt
    #     print(file_url)
    return file_url
    
 之后将后端的处理结果返回前端即可：
 <a class="txtdata" href="http://211.82.97.238:7388/txt/M1坦克.txt"><font size="2">M1坦克.txt</font></a>
 即可实现前端到服务器文件的展示。
 
 具体的渲染样式可以在html中进行修改。加设.class｛｝来修改样式
 
 
```





