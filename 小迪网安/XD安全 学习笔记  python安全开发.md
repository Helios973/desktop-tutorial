# XD安全 学习笔记 | python安全开发

原创 dragonk [0x00实验室](javascript:void(0);) *2021-09-08 12:10*

收录于合集#小迪安全笔记20个

声明

作者：团队成员-dragonk【无名安全团队】 如需转载本实验室的文章，请标明来源即可。文章仅学习安全技术使用，切记请勿做它用，产生的后果与本公众号无关。



本文为Day76-79天 python安全开发的学习笔记。

![img](http://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9wW2QG2oK5uzovZSDDzOZ4gDCbj3HLU1dyvmdC05wDsMu3icGXA4hxDCbBFibmIkGhria16Sa3YlrjQ/300?wx_fmt=png&wxfrom=19)

**0x00实验室**

网络安全从业者 @人无名 便可潜心练剑

128篇原创内容



公众号

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZXbwPRvxus8cOf0QOmmB369UPG2rOLZbianeEy3LsIuib9j6icicwx7DT3g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**一，课程大纲** 

前期信息收集

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZbFZ4Szz0KP8OXcekb0ialveuWmBKKOhb5PLsJ0pTLibibIkIhQHqp9sTQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



Python 学习意义

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZNSZhnVwLsrrNGaibPlo6AXpokD1gquy5HSmlsgR2GkExmWVz94jwrpw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



3. 涉及技术 

Socker,正则，爬虫，框架开发

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ1M3CTVPp5gXs3FTZlCpQmK6kRXGe2z21ibiaYYmcToLXAKbA5ChCBrCg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



5. 流程图

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZay9nYOcEicM4WAW1MlaxNF875rI8RrQGNpYrvRgxTOa25TwS6S8Klzw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



6. 演示案例

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZonOphnAC8FVVibO4e2Q6kQBVYSocCAqzQoyxnonn5uqN83EicmTHxGEw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**二、知识点** 

（一）基础环境 

1. 环境搭建 Pycharm 安装教程（pojie 版） 

https://zhuanlan.zhihu.com/p/379280063 

Pycharm 安装好用得插件 

https://www.bilibili.com/video/BV1ZV411p7H8?from=search&seid=11507145613954022433 

2. 安装库 

方法① 命令行安装：pip install 库名 方法

② pycharm 安装：文件-设置-项目-python 解释器-点击+号即可安装

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZqkMcy3oiah1aTpic8gvro2uLugiavSqHwCicO2dgGdJTdImgVLdZicsXFQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



（二）涉及案例 

（1）查询 IP 

①功能 输入域名如 www.baidu.com;返回解析的 IP 

②原理 Python 中的 socket 模块的 gethosbyname 函数能够实现解析域名 IP 地址的功能 

③代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZYWoE5S35IjwxzyQWVdoRsIZsGOMPicbYb80EppgHTYiapcJxlOoicAEpg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



④运行结果

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZDhkK6AabweKcicT7ns1gh74LyY69bCTGRBKjtHB49hfhV9icw4V2iaABg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



（2）whois 查询

①功能 输入目标地址，能够查询目标的 whois 信息 

②原理 Python 的 whois 模块的 whois 函数能够获取目标的 whois 信息；需要导入 python-whois 模块 

③源码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZXUYtcw6TtaKVBIEU4czKLtYuiaS5nMjTiaqWOjDLlHM8vEXNh5DVz7yA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



④执行结果

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZcbGYumKQRZEIic81VbJ0KP4gg7FGcSRWp5R6XuY9eKZpYVjDEqrCasA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



（3）判断 CDN

①功能 输入目标地址，能够判断目标是否是 CDN 服务器 

②原理 Python 通过 os 库的 system 函数调用执行系统的 nslookup 命令来解析目标地址，如果解析 目标地址的 IP 地址过多，那么说明使用了 CDN 服务器。 

③源码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZGabCWTSu1icqylkh6AuPLZL8AticdCazbT1Lm4CdK7mkN4Sp5t7bjSPQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZut3GDuWyKmO6Ssjk66rVvo9ia5EIn5cvPnn39r735tCDjFTD3ZEictuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



④运行结果

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZZPdibNE4kURj6AQ11RIWv7rMplHMHMsK2Aib4NQupicZBnVESPFzlvTmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



（4）子域名查询 

①实现功能 查询目标的子域名 

②原理 如输入 www.baidu.com；先正则去掉 www;然后加载字典，如内容为 aa;与处理后的 url 进行 拼接，即 aa.baidu.com；然后调用 socket 模块的 gethostbyname 函数来判断该域名是否能够 解析 IP，如果能说明该域名存在，不能则说明不存在。 

③代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZCSgkIbMHJJ6Jic5MpbUuocgkFNkfu04W3cvNesCr0YZS09Soibz5HG7A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ6qUQdGClZne5caSKO8agaDp1XqYI7W0Piaewo65ecQmWFHSt9EMu6rw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



④结果 

注：字典中随便写了几个子域名。aa,bb,www

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZmicEyOtfG56I6M8aEphdUicsDcYicZrQwZwSoHMxYO0ibnV8vXM6zriabKg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



（5）端口扫描 

①实现功能 能跑判断对应端口是否开放 

②原理 Socket 模块的 socket 函数，其用法及介绍如下

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZsicm8dy412Ah8k9Z5UibQiabfWUbDHy5A6iaB5MIoOiaQq5aBgt9lAdSiaYw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



③代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZIdhYrJRmBfeWlZibe1dChuuw15amwibU26jfZ8GicIibtOlupvH7AGIphg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



④运行结果

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZcuXwj1flyVN7Zk6iaoUYjcOKBJ6MUPW5O3lYJpBwSJZb0m7M1SVhXdw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



(6) 系统判断 

基于第三方脚本判断

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZIHessM1VDDk0cK4jNh1Krq7ueLRibHIBODkqr8LtALuiaU59ehaicgib2Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)


三、代码汇总

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZvA9iae9yq6kfOorjJSNMtcwiaEicC0MnpOSvqsh1tziaEhHAogOzXCfq2A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZXaNxkD2pfautPUMsiaM6HEWdgcyOicYPX4mCFlB0vMCEaQDCaGa4dfSg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZRQUEO42Ps9TGSOmrBVrAunicgUpJmxiaCrlnvRDGzSCmblXb88bgPXuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZwia1rZYCs5n7cJaTqJlSLYR5Tic9yMyrgVCicRcribmTuH3iav4HRLib552Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZibHwsEvFVoCKvuAbxXibz8ljLaqQicdicN0bwWQxIesQ3EPwZxSsb1Jxmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZflqK0paykEibuZlGPvB8wLFLQoQEmf0ES9NxCOzwfYzgg0FRibOiaC9RQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





四、总结 

①由需求去搜索对应的功能以及实现方式



------

一、课程大纲 

1.涉及技术 REQUEST 爬虫技术 LXML 数据提取 异常处理 fofa 使用说明 

2.学习目的 掌握和利用公开或者 0day 漏洞进行批量化的收集和验证脚本开发 

3.演示案例

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZjKyibCoZzEI3RK5ptt1MQaAwksWicBibTf5SCKlHldAXZQMfVCMhg0QyA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



二、课程内容 

案例一+案例二

1. 漏洞信息 

（1）漏洞名称 应用服务器 glassfish 任意文件读取 

（2）漏洞 poc

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ7oTfZDLmSHdnssdhS7iahcU1jicH8ibfamSAiafLOHFdAiaBcZK6gZiabVQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

注意：Linux 时读取/etc.passwd 文件，windows 则是其他文件，如 windows/win.ini



2. 编写思路 

（1）实现功能 批量的验证是否存在 glassfish 任意文件读取漏洞 

（2）实现思路 

  ①筛选出存在 glassfish 的服务器 IP 具体实现：>>>借助 fofa 搜索，搜索语法为"glassfish" && port="4848"; >>>通过爬虫爬取 fofa 搜索的全部结果；>>>通过 lxml 库提取爬取到的内容中的地址信息。 

  ②批量验证存在 glassfish 的应用是否存在任意文件读取漏洞 两个 poc;分别对应 linux 和 windows 的 读取从 fofa 输出的结果，将漏洞 poc 中的地址进行替换，发起 get 请求，根据请求的响应状 态码来判断是否存在漏洞。 

3.注意事项 

  ①读取文件时 windows 和 Linux 下的文件是不同的 

  ②爬虫爬取 fofa 的输出结果编码成 utf-8，看起来更容易 

  ③爬取 fofa 后面的内容时，需要将登录的 cookie 信息放入请求头中，cookie 从浏览器中获取



4.涉及知识点 

（1）request 相关 

requests 模块 Request 支持 HTTP 连接保持和连接池，支持使用 cookie 保持会话，支持文件上 传，支持自动响应内容的编码，支持国际化的 URL 和 POST 数据自动编码；使用 Requests 可以完成浏览器可有的任何操作

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZlU38sibwj8uReWheP4qkLn0w2kylKu561k7ht41eTa85w7t6ribiclj1A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

（2）文件读取 

①参数： 

w:写入模式；如果文件已经存在，清空文件内容；如果不存在，创建文件 

x:写入模式；如果文件已经存在，抛出异常；如果不存在，创建文件并写入内容

a:追加模式；不覆盖文件的原始内容



②函数 

f.write('hello') 写入 hello 

f.close() 关闭文件 

f.strip() 去掉换行（否则在读取文件内容并显示的时候， 每一行都会有多余的换行） 

f.read(10) 读取 10 个字节 

f.readline() 读取一行（也可以跟数字）



5. 实现代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZdFyLb4gyYjtkItBGlopx5yvZ605rxkea8uSc3s8xtMGt9cibTN8kFbA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ0yQCia3DCrYLficn6vpibRaombGWZ3qmoicL5mSzaywESXBlpYMsKqR0YQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZFS8HibxkbWIzhzeNtxJbcYGtJNicWZy0ZOMztK3PQo4eiaSibHblnCb7iaA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZwxib7mVZB8ejJicAaWr1X5kDicroXQ49JNpZsCzfvJBYdTuryaD9IiceYQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ4SCqmQ4FHvNibC2WicWIHYTjibMQm4ORfKSv1fJbfEweXRCEy02uy6yyA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZhTE3A6RQukzZgr40ItVGaLlicfgiaAf5WibicJ2fPMAX0r9cVYhvtIxoQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



案例三，教育平台 SRC 信息提取 

使用爬虫爬取页面信息，通过 lxml 提取漏洞信息 读取 10 页的漏洞信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZGtEnZBdKuTSqeNRh1xQXl7gnoJK33A1HO3MYsvaQXXFXhuRUAKKQZA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ9IXiarD2DfpuNblOGGqHqz2kVHuKEPea544YnhzEZ0I9rcjmPzZcR6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZicdYibicC8QRHvwdEuR3NjL1wSv3ib3EXticINWuQuWRbHiajoOu0Np62trg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZLoics8RjzqHQHmicNU5a8qDQQdjsIXX2uyWCBsEgYBomrlYHZV9NzcicQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



演示案例

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZp0hAG3I5vWsy1FcxHU8PMPdbLLcn6fTvYZNFfPDLZ6aStyicicdwIbcg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



Ftplib 模块 

ftp 爆破 

思路需要的参数 IP；端口；用户名；密码字典 （fuzz 字典） 

案例 1，ftp 爆破登录 

1. 思路 

（1）连接 ftp 服务需要输入的内容（参数）有： 

①连接的 IP ②端口（默认 22） ③用户名 ④密码（字典） 

（2）实现 使用 python 的 ftplib 模块可实现 ftp 登录，登入输入参数，IP；端口；用户名；密码。

2. 涉及知识点 

（1）ftplib 模块的使用 ①ftp 接口说明

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ5PhccXz8uWJjWNnTOZUGmibyBrkCN5Ujh3MMMgyB7nZc0dr0b4iauQYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

ftp 常用函数

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZODzHfOiaHAPCwYV4PyGPkjPgMBS1FuW3b86RsoCzDC1hTWEHyNpaUug/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



（2）python 多线程 用法详解 

- 
- 
- 

```
https://blog.csdn.net/briblue/article/details/85101144 
https://www.cnblogs.com/j6-2/p/4645490.html
```

3. 实现代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZwmCW1Anoey0dXia3bZcrqF1ZnrJS5eNKCiabsJ1w1ahy68gYTmGy84yw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZIIibywiaFka99epCNl7RpHRl6AOK53L6M2cpVSVJ5MCicp87cPy1Sby0Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ0ibL1NwFmh1guIwOoZzZ14fHvlldUBjEibNOA8ibxBf17hBOd4eW8VWhA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



案例3

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZZVDaqc53VRJQyc4CgJu2rCouDXzU9okUpS8NHPFPWYIbup9XUWnzcA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



1. php 异或 

参考 https://blog.csdn.net/qq_41617034/article/details/104441032 

资料 

(1)免杀异或异或一句话木马 <?php $a=(“!”^”@”).’ssert’;$a($_POST[x];?> 其原形为<?php assert($_POST[x]);?> 

原理：!的 ASCII 为 33；@的 ASCII 为 64，二者的值转换成二进制，并且进行异或运算，得出 的二进制结果再转换成 ASCII，该值为 97，查询为 a 通过 fuzz 来生成免杀木马 

思路：不看二进制的异或计算，列出所有 ASCII 值<127 的的组合（127x127），将这些组合 放入一句话木马中，然后测试该 payload 是否成功，写入到网站根目录；发起 requests 请求， 看返回内容，能够执行的文件返回的内容和不能执行的文件返回的内容是不一样的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZ4y52eYaCFQvSa0wt7aoLCVAMnkMZSKgwet2jzjMsp2y4Dd7ypU1tGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZdeWXC2sPCxOdDM7h0ZhteDgHmSaicIOcC0bYYyLs1ZIjYcVLVdgiaqPw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZFx2TlS2wBxic74yNruFMY9eAdlyyg13W8QlTLVSThm8rP2Lnoo00JJw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZlI4XibbfQvrN4lZo4OAX8tg1AicACdovPx9jyeGdjaAcMToSvMjmUu2A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



4. 更多思路 <?php assert($_POST[x]);?>这种一句话的内容都可以通过异或去生成，或者以其它免杀方式 结合异或，代码变异



课程大纲 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZHwBm6OFfjTFY3LNj0WkhExxT8qEOIiaZhCSalq2uQIOyWqhBulBcFXw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZvLoLjiaTafPkWJyUjOJHibKVTAMWiaWib29W10e8bibRFl6aIBYA1H0VfzQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZmqicJ1Z41P9gF1V0C7x9rWuOPCkDAXnN9JY0Sg65XicHkVXXmG0WSyRQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

演示案例

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9nQjFnFs181AmQTEkpV0tZn961gnFChicz9vVLDyHIaVFRmicJWRqkTDpYVUqAeZFKGdhxwYsEWaCw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



二、课程内容 

案例一-SqlmapAPI 调用实现自动化 SQL 注入安全检测 需求：调用 API 接口，实现批量扫描，自动化扫描等功能（sqlmap 本 身是没有的） 

案例：前期通过信息收集拿到大量的 URL 地址，这个时候可以配合 SqlmapAPI 接口进行批量的 SQL 注入检测（SRC 挖掘），就不需要 再人工挨个挨个去操作了。 

目的：利用 sqlmapapi 接口实现批量 URL 注入安全检测 

开发当前项目过程：（利用 sqlmapapi 接口实现批量 URL 注入安全 检测） 

1.创建新任务记录任务 ID @get("/task/new") 

2.设置任务 ID 扫描信息 @post("/option/<taskid>/set ")

3.开始扫描对应 ID 任务@post("/scan/<taskid>/start") 

4.读取扫描状态判断结果 @get("/scan/<taskid>/status") 

5.如果结束删除 ID 并获取结果 @get("/task/<taskid>/delete") 

6.扫描结果查看@get("/scan/<taskid>/data") 

- 

```
参考：https://www.freebuf.com/articles/web/204875.html
```

案例 3-Pocsuite3 漏扫框架二次开发 POC/EXP 引入使用 

- 

```
参考：https://www.freebuf.com/articles/people/162868.html
```



开发当前项目过程：（利用已知框架增加引入最新或内部的 EXP 进 行安全检测） 

1.熟悉 Pocsuite3 项目使用及介绍 

2.熟悉使用命令及代码文件对应情况 

3.选取 Glassfish 漏洞进行编写测试 

4.参考自带漏洞模版代码模仿写法测试 python cli.py -u x.x.x.x -r Glassfish.py --verify 

- 
- 
- 
- 

```
涉及资源 https://github.com/knownsec/pocsuite/ https://www.freebuf.com/articles/web/204875.html https://www.freebuf.com/articles/people/162868.html
```