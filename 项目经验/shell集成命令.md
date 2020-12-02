任务：shell集成linux命令
- 任务来源： 实验室汇总知识图谱系统。
- 任务内容： 将linux命令进行集成化，使用一条shell命令即可解决之前的复杂的目录切换，命令输入的问题。

问题：通过windows编辑的shell文件到linux会报错，提示空行'\r‘ ，这个问题是dos和unix字符的不兼容造成的。
处理方式： 在linux上 

```shell
apt-get install dos2unix
安装结束后
dos2unix XXX.sh   
这样讲就可以将文件字符进行统一。
```

注意！ 这里我也尝试过使用统一utf-8，但是并没有任何效果。

![image-20201202211559614](https://cdn.jsdelivr.net/gh/hodge-ge/imgbed/20201202211608.png)



