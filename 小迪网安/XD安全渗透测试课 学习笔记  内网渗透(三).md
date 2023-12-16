# XD安全渗透测试课 学习笔记 | 内网渗透(三)

原创 耳鼠 [0x00实验室](javascript:void(0);) *2021-08-12 09:10*

收录于合集#小迪安全笔记20个

  本文来源于团队成员耳鼠的学习笔记，全系列定期更新。



------



***往期回顾：\***



day58-64：[XD安全渗透测试 学习笔记 |  提权阶段](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247483877&idx=2&sn=35f4bdce1d4043219dbbb67420bb5818&chksm=cfd87e1af8aff70c74e6a5541d7b120548ac3242a320083a81f4b0ae3971d60b2903e7245705&scene=21#wechat_redirect)



day65-66：[XD安全渗透测试 学习笔记 |  内网渗透(一)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484046&idx=1&sn=85b730dafd28d0e798b8c70fe8130582&chksm=cfd87d71f8aff4672c1c0d254342b6e63f026b825be225e8af7fa30f364cf49bbdb384b42b0f&scene=21#wechat_redirect)



day67-68：[XD安全渗透测试 学习笔记 |  内网渗透(二)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484112&idx=1&sn=036ffcfd03b4a4ee94f6f787cd7986a7&chksm=cfd87d2ff8aff439d9c2a89c2054b471a05c800e7a3d7ee605ef642b3321cab54b1423243454&scene=21#wechat_redirect)



------



**day69.域横向 CobaltStrike&SPN&RDP**



案例演示

案例1-域横向移动RDP传递-Mimikatz、

  除了上述讲到的IPC,WMI,SMB等协议的链接外,获取到的明文密码或HASH密文也可以通过RDP协议进行链接操作。



RDP协议连接:判断对方远程桌面是否开启(3389端口)

RDP明文密码链接

- 
- 
- 

```
Windows:mstscmstsc.exe /console /v192.168.3.21 /adminlinux:rdesktop 192.168.3.21:3389
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp5AIMdPvOoZyJ24tEFVdY8V5Xia0MYnefZvuIm94naKkDTmLFI8cF4RA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



案例2-域横向移动SPN服务,探针,请求,破解,重写

- 

```
https://www.cnblogs.com/backlion/p/8082623.htmo
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpxB7NFkavtet1UHVjWaLHXD2abwEKia7rQSosmkhq8SBZYDZ7bjURiaCQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- 
- 
- 

```
#探针setspn -q */*setspn -q */* | finder "MSSQL"
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpDxoXuvHoz6VkJfa3Fa5icGI3BlQxLkjLqp17VyyzSHMOs9hic1BfS2Vg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpegYqU05B5aic4wKsujyaNKPFyicC4DsKcdlYpbpdWu2VdFezxNhdQ9zA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



请求:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpt2EG6ic01MrHVCcBFavFOFuCtdib2Kv6iaUjjkTyciancrn4biaM0niaSJYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpribDUAbM7DDzGnnzXr0eIxvJpRias85ud8ffcd2nYyCTv2eHXdtj1BibQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



导出

mimikatz.exe "kerberos::ask /target:xxxxx"

用mimi导出刚才请求生成的票据

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpyicE5C9cIvtwQ9UEdnzLlToLrxulrFs6iatFfHyG1LaLjywIaJicicppicA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



破解

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpRJCvdnkMfOaImpIRA7NPRqxekoSxAwhW1CbImOI1fpyh7cM2vEu2fg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

破解得到一个密码Admin12345

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpvvEYkvFc0MZu5qVA4ODBcnGAmXj1WAIjTIR2wibg0yU5TrPBF4ARhgw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



域控主机的环境信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpY41tzYRgIwZGgw5Jp7vFg2d2EfbnelnIr3yc0ibrVGMhF2Y9g9icK2iaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现的确获得sqlserver密码



重写

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp8nKX6diaDm0DibT9RSjhvxL7VBib4aA3yN8EmBUyzDxqjeFiaGKAK8quMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



域横向移动测试流程一把梭-CS初体验



大概流程:

启动-配置-监听-执行-上线-提权-信息收集(网络,凭证,定位等)-渗透

- 1.关于启动及配置讲解
- 2.关于提权及插件加载
- 3.关于信息收集命令讲解
- 4.关于视图自动化功能讲解



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpms02XIYmZNGLyrFw2C8DEf02jibAPAFul4ajo3OApmWV9NrdZjaFeUw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



客户端连接团队服务器

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpjVXVpDyibkh0rtsFto4lRmopXyJ939GLXYfb1WkvycKjhwzYHE0wtQA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



启动团队服务器,设定密码,密码再客户端连接时要验证

点击客户端连接

进入到了CS界面

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpo5n8unXu3e5TDIUpHkW7ibkdTNEOKqfzibKvqlYDibmj8ypnPrHJpI06Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



配置监听器可以区分那个是自己的,监听器就是配置一个木⻢传输的管道



配置监听器

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpfeoiaCuMFw5wo6C8uwqmjLBEvK1nibRJibkr5J3cTmbT40CnQsEscg01A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpWQfPibJhYUbI2cg0EvCZVVn69oiaTDFtmHd46x7OO3PIxEuYexNVYZmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击ADD可以增加监听器,并且监听器支持多种协议

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpf2XhPj3jxoibtrcYWS97X2juaBlVxbhmspDSnicO4nDfNbicepAghUCFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击save保存

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpoeHkicXrj6VIhv8emYLauSKANBnv8SE0cZqdIGKEgSnGHCI98XB7J2w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



监听状态

生成后⻔,有多种后⻔,我们这里选择Windows

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpe4ZNwjR8Mry68PMNl8liczAtpEblWHlltKSmqz4MkCneFOWvSjXLWFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpRlibKxCUY5CLbD6t41Rvn03FPD5M7l4g0LGD2mzBgPv9nO4ictvnKLfw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

绑定监听器,这个后⻔触发,就从当前的监听器走

上传到目标服务器

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJps4PBdyialBHJPwIqib27bchUVWe4bia3Q2f83ALVoFSMHVqrIKHAABnwA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



后⻔执行后,cs客户端上线了目标服务器

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpBgiaSCc1dKJgs6Cn2WPuiaylqX1VPf5NwiaNkl3c7mvaSIOicQEG4PAPlg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



点击视图,可以看到许多有用的信息,例如当前用户和已知的拓扑图

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp7DJVYpgmSlDn8yVhQD5j9FmjNJmib8EqW2HtfZzOVcuA5DBbxYczuXA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



接下来进行提权操作



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpIbTNfqEg15tIh20Z9Tr3HIxcDx1vXwbwsZrz2bA6lqQia4aEoSLDejQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp6RgBOwmr5sfuT5Y1gYjORMp4ianCYicZohZFvNaeR6M9TSpLILWia5nWQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



自带的只有两种提权方式

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpNL67t1ibkaqM5aC08u6wLtIXrgOvjyJEMibq5xicI16yMQRiaZYA1D0klQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



我们可以加载插件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpRqmjXL68y6hedImYaCU4Ulkic0gLt1EcqoGv8CI8jVAgUvksvqf24ZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



上传插件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpf5H8fP4GapKClwLZibws6vEKmCx0u87dSd2xsYqWyvGfo4yZVvuk95w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpgZJvecDONhEB9IMr1YvfEwsDwZiagoLmW0bHgiaGribzkZh3gBJicgAYtw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpe4uoib9cJl6uPkSh19p2hDwjhG3rUGz5kcLe9rIRSKqjYjHAyS6Lefw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





载入成功后,发现提权方式变多了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp6fqYChVMlLpNAsyiblNV7AOG2UmxqZ1Shr3PEhlibTmmib2tSicWN8mnPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



选中payload和监听器

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp5XHMAfof9p2xCO65eZHefiaTaZLYvqRz2HMPslZwuSiclJXWqceeian5w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

开始攻击,如果速度过慢,我们可以重新设置sleep

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpmaicZiaoZgcmn8ribkeTjB5EsW3O0nABGQF0qgj7c299GANcCN3Os9vYw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



出现了一个system,提权成功

点击insert进入终端执行命令

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpMXkxV96xAicOSYLLl0dXVWxPh9ib6ricJSHUd62WMJpyVocjZicDibeIhwQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

可以输入help来查看有那些命令

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpROvl8o97NNkiajyM4gUqQ7TsuJ5Mu7gD1gBLxic4Z7mvRBdt1QnG0YYQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

信息收集

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpBwK9NZvPjbllz39KBvOm9OWr8tUVNjmmgFQ3vuiaGO6iaf0UUNKu9Kgg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

探针当前网络环境

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpwPib1iaPJDY2SZLeM1xN4HwPGNj3Yr2RP7P6Qj1gtlgPsEZial69Pufhw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击view->Targets

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpWP2MWEUbEp2giaJXGhtKiaO1PDzFC3Wc9IhkG7KuhgpbpYGXaIdpYm4A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

获得当前域控列表

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpJcIWxYM7iaRialNQHISFib9uia4RZb8IAQgibHZzqiaYTjq03e5jYicDsib4bA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

通过mimi获得的密码可以列表化

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp4hzTSbLo7BOiaE3icOt45Wm1arCMzA3Bpm8IXnHh83yAPfVacswGSDng/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpCqXqLGN9le5ib6xnicfyAngDDaT7DZd97Iqy2LYeJNM7rfPqXAIYGnLQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



选中主机来进行攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpIX9dNrxgRVlZlzTOXNFjicdMls1Lm3Dp0wLS5QmSOpq6iaLVwORTROUA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpiaRiaia7A6ficcSwlUCFiaapFVBjMTPCT9vgy48sDCNxjrgLB54ZsQ2Vy9w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



尝试连接

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp2G7Mov19eJv8L6aiciae0CpyauF52ia6R9Q2U7mRRKE81u0zkDVQ49fsA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpwConVpMPYjAf2asYhJbSavysn4IQWmDQ9LDFetptbFGYKbdwKCZTqg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



代理问题

自主工具

可以上传sadon



------

**day70.域横向内网漫游 Socks 代理隧道技术**

必要基础知识点:

- 1.内外网简单知识
- 2.内网1和内网2通信问题

可以用代理解决

- 3.正向反向协议通信连接问题

​    正向:指正方向连接,控制端连接被控制端

​    反向:指反方向连接,被控制端连接控制端

​    假如控制端是公网,被控是内网,如果,你直接去找在内网被控的是无法找到的。可以让被控端去找控制端

- 4.内网穿透地理隧道技术说明

​    隧道主要是解决流量分析工具,流量监控工具,防火墙相关过滤

​    代理主要解决网络连通性问题



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpMQh801jEkSYAt1CyjWibic1nIw3zX3g2GYbQKR7mlaw8biaBm7Lnpbgew/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



演示案例:

  内网穿透Ngrok测试演示-两个内网通讯上线



实验环境:两个不同的内网(有网络)实现穿透控制

1. 注册-购买-填写-确认http://www.ngrok.cc/协议:http 本地端口:192.168.76.132:4444

2. 测试:内网1执行后⻔-免费主机处理-内网2监听-内网2接收器

   ./sunny clientid id

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpvNjSA8CBpevPz761ySCW7aCfIaq1noSKzez2VoJrF6Ioicgujwza7pA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpSEUwgJsNhibp27icYVZGRSADia2dmNZ5LLibSrRCTowqemYSooZGTIkiaog/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp4cicL3W0ibaWLI5jTjXRdicoLvhv5CE5tnrIjzSic4cpG2qmROHgICbbBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



攻击机是一台kali,在上面下载ngrok并且打开它,clientid为注册后给的id

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpsXZGgonwmiccUFqeLt2jXicJgAC4bdgxuY4UgCjqlw89iaoiauhv1ibGnyA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpyN31FQ1AEJfbaPibv8a4M6VqkFGS4lZyncIU242oOoKAibzib8IzmU7cg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



目标机只要访问xioadiisec.free.idcfengye.com就可以连接到攻击机kali

msf生成后⻔

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpershCSmLQsLzVicThJpyyOTpUdr9Fp3SU6dEkb3wH6ljkGSp8Osz2OQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



目标机是win7,将msf的木⻢上传到目标机,在kali上监听

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpdkQolofF5iaJWibJRRMk0D25F9dHwAhmzK8aJkjZ8L7s1gZuyDJRamvg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在win7的目标机上执行后⻔,kali收到会话

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpTKdp7QSUU0Inr0Qk4ibTcXg1klEgsiarvPEBb1MhZpeZkd9Y8QialXiaog/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



并且在sunny上也看到流量在传递

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpIfQ1BkpgGduUbniblZh5q9j9L7zllq2KljxXsamjOSiaib5CpicLkQszgw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**内网穿透Frp自建跳板测试-两个内网通讯上线**

自行搭建,方便修改,成本低,使用多样化,

1.服务端-下载-解压-修改-启动(阿里云主机记得修改安全组配置出入口)

服务器修改配置文件frps.ini:

[common]bind_port = 6677

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpicmRPzictoHCTMGRtaDA8zOp4xRSMJZibUEselpKhnNqeKfiahxn5Z08Ag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- 

```
启动服务端: ./frps -c ./frps.ini
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpLVU53Trnm3wBM5suH7fCpKZlLFsukDfdsdicLPYeHeDcILYUth3QDmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2.控制端-下载-解压-修改-启动

控制端修改配置文件

- 
- 
- 
- 
- 
- 
- 
- 
- 
- 

```
[common]server_addr=你的云主机ipserver_port=6677 #frpc工作端口,必须和上面保持一致[msf]type = tcplocal_ip = 127.0.0.1local_port=5555#转发给本机的5555remote_port=6000#服务端用6000端口转发给本机
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJprZN1RYeJhvnEL6IFSQBzSfhrBp2K1kJAJSxzK4Np8knmAcUu9lZjjQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- 

```
启动客户端 ./frpc -c ./frpc.ini
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpkjhvMuwa1DvNTWe9KxXxYCibfAvlsHpakdey9GYrib1f5z9lwgcWfaLg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpgIZ7GnoFrHwEs59HPZJ4K04dUibBlMiatApOOpaPtvsjEu8ypqEsQicFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



MSF生成木马上传到目标机器：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpibqZlMGhR8s18VhicicHtYdtHMSC1WpogasGkqGNn2F642gWAziboRd1icw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



MSF器监听：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpYX7OicIIO3kcX7MLWCm9zXkXmSALIETCTyUmkZ39KpmhUK7ibibFL5DPg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**CFS三层内网漫游安全测试演练-某CTF线下2019**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpRwGicZpKsAItDFBFVB3hYT1PgJicLXde7PpeSvAKh0K6W4Uj8Mg4zJTQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpRCTFJ2kSbcS3yLa5ibf7znrD2KMOa2EKZ6AZwhkqACEUh5ysyEXcnYQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpmx5hfCST8D0gAiaN9CgIlS7tLLSRYWpkK4bgMgyS9QWpo0MAX6Gp4dw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpibrGIj4EYmnoDhAODNhO5n7wkrTwlQdTib3xEqJQyUrgrZz1upiaBbQXw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpdthHUxemBCv6plZFXgfiaHDnqMw9qaPbtxyVp3w1njrxicWKoSASyJsQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpO3YF7lOjZ4LATjYs2xwqRJlDxxHhOy49rgVapouCpib40rCE4xyFzUA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpjFnBkmiaiaTyeZe61LnbxIpicvwkiamGhZ07pEq581NMJqmUE3IBlRaRjw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpEia4aDj3dmasibkVUZfXkCWBMsQ7Oiah6qKgLiawMwtO6cqvux3rmnZ6UQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



开始

  第一台主机的192.168.75.148我们可以直接访问,通过端口扫描,得知开启了80端口,访问该端口得到以下内容

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpIiam7qu2z23pI39jgXNYJblm26Azk98tKvjyfu06S55gnqNEc0Abzicw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  

使用thinkphp的攻击工具

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpdCHVGdkWWKmhrXWzDNqqU7VEEKicsRnz6o41x50of3iahkkVqClIXfmA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



得到一个命令执行漏洞,写入webshell在当前目录为2.php

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp4BD5pS5RjMJ8hjf1boficAgZzztCuu9vZgHtAfgaibzyBaDNLfNX9adQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

使用后⻔管理工具进行连接



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpglGyGicaD4a2xKibrnub8bicjgEEEsHLZ4VAPbiarNIEo0jzeN8oufImew/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpG8jia0iciajpJTZdm2qKGcbqouNKNsNPL6bPcia1mckbiaqM6iaiaib5vKvTQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



我们要将它作为一个跳板,使用msf或者CS

现在使用攻击机kali

使用msf生成一个Linux后⻔

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp3c9xOabkZmibpOQjOmmwic2jX5ibTe7E1qcU4zSLkT6HxjaoViaLBrERxQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

msf监听

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpxuQpuFcwIHkLwPjn9PY19K2jKvSwGdgQQnc2dVwqWCuR7fPiazQrxQA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



将木⻢通过webshell上传至目标主机,执行

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpmA7RE0Dic51VIQ6VR8mwVeM1dmyZsy4JldE0dT7w85ZwPb7G17H6K7Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



msf接受到会话

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpUYe2sTrsd3IWNvzZvZBA7Micol7C1X8iczEscaJzCVlrConNQoicgCJvw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在msf和cs上有自带的代理

信息收集,获取网卡信息,看是否有内网

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpLHUpg79TjdMNtUJTUXzxzOE4qsvxcibwrPqfHGpkElXDztSxY26qNfA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

除了.76外还有一张.22的网卡,现在无法与他连接

查看路由,看是否和他建立路由规则

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpIKVNPCicgu9jKdsyWgc3gGRQfLNO5vicDvnCicHgrJibQqTkA9NnsL7KAQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



当前路由为空,添加路由地址,再次查看路由

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpANkkF7h0KBultWPzb3rOaZFv6WwOhAHNzPSSAS5sqJOgskovyeeqGQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  现在应该可以和.22进行通信,但只能在msf会话上操作,无法攻击。我们可以使用msf在本地开启代理

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpSCEX9kztZj9QaO1ZY3xP0VVEl7ymnicVmCaMQYqGsxGmBMxWjUAg6jA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



这里应该是设置的srvport

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp1cRETkdNOfFcAAmyk3AlwBzHmIgq8RIRqgTzHAOKh43jKYF33Oh49g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



开始利用代理端口扫描

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpntCRxoNwba1eNLAjBMu5iaK2bRoVUTws3DJhsDYkof1dozyK4MBSrVw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



  扫到.22.128的80端口,直接用浏览器无法打开,配置浏览器代理,直接选择Socks v4.

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpZ44t6HgJTNSYTlK7sGLtw5Ollf0aRfMhMXmLVic3EicNbE4lYEPS5iaOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以访问

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpIy9K29VYlxtbjQEHjszYXUnCiaKTBgsrxvRVfuyyOAeeowNOyXR1mtw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



源码中告诉漏洞在哪

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJprgBhoEpya2F66bvibDwSSGj5WEsJStIxFpRNEudiaUbtyibTUcB2mvibDA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

进行SQL注入得到密码账户



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpssBkNO9PibjbFE6ibtkiaoNuzKOaPNQ0WAanCv0DAJsqicBias7iatRLldLw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

通过robots.txt得到登录地址,登录后台



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpZO8Caw52tM64aDoVoqdAwDB9wRQyguKB8ZicuGqSMTbVSIn8qAyia4YA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

可以通过后台功能得到webshell,可以往里面写websehll

观察网站url查找规律





用工具连接后⻔。这里推荐蚁剑,可以设置代理

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpvCz2tyeQh8qIrhS12G1hcLGegCWdylzCSgu0tDjlEkeD5mYGz2xB2Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJplKiaWGv8ENvUWwQ7ymAh4KyPAjvCvIOuPficmSwOtXl5P8gOHjM7HO7Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



也可以使用Proxifier全局代理工具,

还有SocksCap64进程代理工具。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpLyc9w6RAg42ia8eba6GOKgial94XKSmt1e4y2uHkjSNY4QUpowRhjKuQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



加载代理,然后再将webshell管理工具在代理隧道中允许

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpzaN9ibdTGqh7iceDiaCVozyT8bb4yPwy4icOqcz01sZVDVMGHmERv7QmWA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

使用msf生成正向后⻔绑定本机3333端口,然后再去寻找目标机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpcQsh6ZJWjuHGeMtN42MWsuShMrgRHibxGKGVcvibHLALO40zSqWDGkYw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

将木⻢通过websell上传至目标主机,msf监听

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpupJT03CRoibg1866HKSibESb0DyvtOL1mxxzia8uTiaibiaX6svr7UeGFeTA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

目标机执行木⻢,msf上线

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpTGGQEttThIpC2l0cb4M3ibUXHWcqlv2J0sjiaD5wGqOqRguArzZ67tiaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

得到网络环境,添加路由

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpLnwpLBPq1mJUXe6cBY62bAMAU2JV0HxuWMxyQVNqdjUHlsVBtleJqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJpAWRNEpz9Mr37kG3iaHEERia85kic6gMFymUv6ncvibJiax8Oic9ofAdOufWw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



重复之前的操作 ，使用nmap -script=all扫描漏洞

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibXAqM8ibM1I5CkdnJntwicJp1nzvu3ttZSI1icgQUxD9md58Gq7hsLWZvKkgRIGhskSHHCKyNj6jUlA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

本系列的教程将会定期更新，麻烦点个关注吧。

![img](http://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9wW2QG2oK5uzovZSDDzOZ4gDCbj3HLU1dyvmdC05wDsMu3icGXA4hxDCbBFibmIkGhria16Sa3YlrjQ/300?wx_fmt=png&wxfrom=19)

**0x00实验室**

网络安全从业者 @人无名 便可潜心练剑

128篇原创内容



公众号