# XD安全渗透 学习笔记 | XSS漏洞阶段

原创 笙文 [0x00实验室](javascript:void(0);) *2021-08-18 20:50*

收录于合集#小迪安全笔记20个

  本文来源于团队成员笙文的个人笔记，本系列会定期更新直到完结。麻烦点个关注获取更多信安知识更新。



*本文是day25-28天四天的学习笔记，XSS漏洞详解。*

往期更新：[XD安全渗透 学习笔记 ](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484353&idx=1&sn=41ebcb2d6503d0a1cbf8ba59938db15f&chksm=cfd87c3ef8aff5285c0c6614a4be30c9f944d02ad32cfe150c5c52e6e81660518fa515532d5a&scene=21#wechat_redirect)*
*

![img](http://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9wW2QG2oK5uzovZSDDzOZ4gDCbj3HLU1dyvmdC05wDsMu3icGXA4hxDCbBFibmIkGhria16Sa3YlrjQ/300?wx_fmt=png&wxfrom=19)

## **0x00实验室**

网络安全从业者 @人无名 便可潜心练剑

128篇原创内容



公众号

------



**Day25.xss****跨站之原理分类及攻击手法**

原理

  XSS 属于被动式的攻击。攻击者先构造一个跨站页面，利用script、、等各种方式使得用户浏览这个页面时，触发对被攻击站点的http 请求。此时，如果被攻击者如果已经在被攻击站点登录，就会持有该站点cookie。这样该站点会认为被攻击者发起了一个http 请求。而实际上这个请求是在被攻击者不知情的情况下发起的，由此攻击者在一定程度上达到了冒充被攻击者的目的。精心的构造这个攻击请求，可以达到冒充发文，夺取权限等等多个攻击目的。在常见的攻击实例中，这个请求是通过script 来发起的，因此被称为Cross Site Script。攻击Yahoo Mail 的Yamanner 蠕虫是一个著名的XSS 攻击实例。YahooMail 系统有一个漏洞，当用户在web 上察看信件时，有可能执行到信件内的javascript 代码。病毒可以利用这个漏洞使被攻击用户运行病毒的script。同时Yahoo Mail 系统使用了Ajax技术，这样病毒的script可以很容易的向Yahoo Mail 系统发起ajax 请求，从而得到用户的地址簿，并发送病毒给他人。



**危害**

都是通过js脚本来实现的浏览器内核版本也会影响到js代码的实现

- 1、钓鱼欺骗
- 2、网站挂马
- 3、身份盗用
- 4、盗取网站用户信息
- 5、垃圾信息发送
- 6、劫持用户Web行为
- 7、XSS蠕虫



**分类**

反射型(非持久型)，存储型(持久型)。反射型攻击方式就是把可以执行的 js脚本放到URL参数里面。存储型的攻击方式通过评论的这种方式，加载评论的时候把它写入到评论里面，它被后台存储之后，用户再打开的时候就会执行评论里面的脚本**。

### **存储型**

发包X=123 => x.php =>写到数据库=> x.php=>回显

(1) .评论框中输入script 代码, 一段未经转义过的 JS 代码被插入到页面之后，其他用户浏览的时候也会去执行它。如果是黑客它插一段JS代码，把用户cookie的值发送到指定的服务器上，这样他就能拿到用户的cookie值想干嘛就可以干嘛。我们知道HTTP协议它是没有状态，所以很多网站是通过Cookie去识别用户的，一旦黑客获取到你这个cookie就相当于拥有了你的账户就可以随便使用你这个账号了。这是个什么类型的 xss? 这个是把提交的脚本插入到数据库里面，所以这个是存储型的攻击方式。



发包X=123 => x.php =>回包

(2).有一些后端它是通过URL参数来去获取的，有时候会把脚本放入URL参数里面如：http://test.com/xss/example.php?name=，然后通过邮件方式发送给用户，诱导用户去点击,这就是非存储形式的 XSS



### **DOM****型**

发包x=123 => 本地浏览器静态前段代码=x.phpHTML DOM 定义了访问和操作 HTML 文档的标准方法。DOM 将 HTML 文档表达为树结构。



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JtMsedHw16BYTYn7z4eeRxQiaOG9sDUUFTfibWUyOYiaaRquaTOkylgb8Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



​    W3C 文档对象模型 （DOM） 是中立于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。W3C DOM 标准被分为 3 个不同的部分：核心 DOM - 针对任何结构化文档的标准模型XML DOM - 针对 XML 文档的标准模型HTML DOM - 针对 HTML 文档的标准模型我们主要来看HTML DOM HTML DOM 是：HTML 的标准对象模型HTML 的标准编程接口W3C 标准DOM 节点根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：整个文档是一个文档节点每个 HTML 元素是元素节点HTML 元素内的文本是文本节点每个 HTML 属性是属性节点注释是注释节点。



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JMwrYC80GQLKibwK5vgtwxURcqGNM83D6BND9REeKNcoiaRUrkw5EfJdw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**案例****反射型（****gat****）**

首先我们会用到我们的pikachu靶场

1.这里我们打开反射型的xss靶场输入如果在对话框里面输入不了这么字符我们可以去更改网页的属性

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JxNR8BVl5x7lnYkIwAuo9eSx1GtnicoXDQx0uw5FOK7f16IiczzVXzmFw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



或者在地址栏里面直接输入

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JON3g91xjiazJhFzibGCGPXe1DkIkSPnwEhnLlpZ7QHH6o5L9tPbASTvg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

通过邮件方式发送给用户，诱导用户去点击,这就是非存储形式的 XSS。



**存储型的****XSS**

1. 存储型的xss一般存在于留言的地方但和反射型的最大区别是没打开一次留言都会自动弹出我们的js脚本也会一直攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JdfI3cI2kLSMictGw1WDPNCzDYzvwPYXiaia43e56NgaTrJ1DIN3Ha7FkA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JYSFOZY2Kj4vUYuBQ8xARBlR8ZY89fAmXVmpJ4yvdvqmz7nHpmfUlbQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这个是把提交的脚本插入到数据库里面，所以这个是存储型的攻击方式。





**DOM****型**

DOM型是直接调用前段静态代码#'onclick="alcrt(2)">

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JNwysSicxboXu7844YHVLr8BWRdvjAfGIAkfzokQtBNib9CTZnxeVHrCg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

当我输入111时在数据包中发现

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4JR5lHvV2LtCiciaar1TqFMjl0q2Br8YNxR7GNMpL4KiaXC8CJYpQY2hUrQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

是传输的这个值，但后续继续点击这两个链接最开始所传输的值并没有改变也没有增加其他的数据包

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ94ef1xyKUmddjC9WZBkF4J3VNEq9lXIicsX5doHq9ZbxV0FbxBRayqPosQLg0elnuMw4SMJZzFpSw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

由此可以看出DOM型是通过前段静态代码来实现的也是前段进行注入



**区别**

  DOM是属于用js代码进行处理（可直接通过查看代码进行判断是否属于DOM型）前者是属于后端语言进行数据处理的



------

**什么是****cookie****，和****Session****的区别又是什么**

  Cookie 和 Session都是用来跟踪浏览器用户身份的会话方式，但是两者的应用场景不太一样。Cookie 一般用来保存用户信息 比如

  ①我们在 Cookie 中保存已经登录过得用户信息，下次访问网站的时候页面可以自动帮你填写登录的一些基本信息；

  ②一般的网站都会有保持登录也就是说下次你再访问网站的时候就不需要重新登录了，这是因为用户登录的时候我们可以存放了一个 Token 在 Cookie 中，下次登录的时候只需要根据 Token 值来查找用户即可(为了安全考虑，重新登录一般要将 Token 重写)；

  ③登录一次网站后访问网站其他页面不需要重新登录。Session 的主要作用就是通过服务端记录用户的状态。典型的场景是购物车，当你要添加商品到购物车的时候，系统不知道是哪个用户操作的，因为 HTTP 协议是无状态的。服务端给特定的用户创建特定的Session 之后就可以标识这个用户并且跟踪这个用户了。Cookie 数据保存在客户端(浏览器端)，Session 数据保存在服务器端。Cookie 存储在客户端中，而Session存储在服务器上，相对来说 Session 安全性更高。如果使用 Cookie的一些敏感信息不要写入 Cookie 中，最好能将 Cookie 信息加密然后使用到的时候再去服务器端解密。

**
**

**常用测试语句语句**

- 
- 
- 
- 
- 

```
<script language='javascript'>alert('test！');</script> <script>alert('test')</script>  <svg/onload=alert(1)> "> <svg/onload=alert(1)// onfocus=javascript:alert(2)  " onmouseover="prompt('xss')" bad=
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibkkyDUexwUCJZjGvoLCgqO5AjMgqStgx70q8iajibicYogftzLfumFHcCQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibV9YF7uHB9cQlU94JJK14ltrHjxHE7gVGPichWML2tqxKMSWE1R4wWBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibbiaJxXibiaOyjEzyJdp1dRiadltGgVX2EwkJicTkN8BSyOdYtgZnVvjzDtA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

**Day26.xss****跨站之订单及****Shell****箱子反杀**

***\*订单系统\****

**1.下载军锋真人cs野战的源码，http://js.down.chinaz.com/201202/jfdd_v2.0.zip**

**2本系统是单用户挂号查询系统，用户名和密码可以在初始化的时候自己设定，用户名和密码保存在config.php中，安装完可以自行修改。安装前,请必须确认根目录的config.php文件可写然后在地址栏目输入安装地址 127.0.0.1/jfdd/install.php 一步步的安装.**

**3.通过部署我们发现我们可以在订单界面输入我们的xss脚本，然后再由管理员去打开，实现我们的xss渗透**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibFpmf7nv3A7u1SibTKbiaEElKLaqrTOgpF4bJEOWGU8CJBMBjE6EsFzOw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibVMaeVhQl1fpj146736lQ91IZc2UFLpLNkOicZNLq8nRWy3Ytl7PaTNA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这里我们用管理员去查看订单时，我们的xss脚本就已经插入了进去

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibQCDJkeqF9GpQScTkdQIFXYXWoX6VOciaWYzgPUhgUJiaSPzEDWPXFdjw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



接下来我们使用网上的xss平台我们去获取管理员的cookie

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibIL96tUbsCQMTTAwbPT9CgTnOkiapHqNoBd3poCtVSofBTbHxf8vmlHw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

现在平台上是没有任何的数据的接来下我们就去前段插入代码





![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibQJyBib0YdW7y5OVfmxHOFQAc79KOXQ6jKtLxYIXicw5jDVzKHyib1Qejg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibibWrWziaPoSg0SVOKJUMzzy1bmbIY5lVic2oNucn8BO2XIWh1nk0iaib4QQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这是我们发现我们的xss脚本已经插入了进去并且用管理员打开

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4iblHfVoNkcibDCNvNcxPTiatNxjFXzZvq6DkiaCG8lmbcibIceAQJXB0v7rQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



我们再去xss平台查看是否已经截取成功管理员的cookie并且带有页面的截图

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibX1FbsAiclBALzfG8vh553uB1GVXjC817uLBUnASNZS5sEAibkREeWavQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后我们使用postman进行连接

当我们不使用cookie登录返回的是这个内容

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib5h2wqfhicqO0Ty8Al5JLVIbfGJDBUwlU5nGog1PIQC9Y5bIpiblr2srA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



  我们使用cookie直接就是返回了我们的后台，所以我们只要获取到了管理员cookie和后台地址就可以利用cookie进行登录

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibJLAxnvT7AicBnsdpWDmbamCV4lh58shuHwib2H4wXSfEjpHzyicIJ4Ktg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**shell****箱子**

利用在webshell程序中，植入后门，形成“黑吃黑”，用有后门的木马入侵的网站也就被木马制造者利用。

1. 搭建一个asp的服务器，我们这里选用小旋风进行搭建http://lt.yx12345.com:90/yasuobao/jyx12345xiaoxuanfenglinshiaspfuwuqiminiban.zip下载完成后解压到虚拟机里面。

2. 2.默认端口为80 如果80端口被占用或不想用80可用记事本打开mian.boX修改至你要的端口。

   

   ![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibd4j2MC5QKDzz3VCyUic8yNNJBr3flicu98x4OT2YQY7WWHgpQtuNELlA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



3.将下载好的webshull箱子放在搭建好的服务器里面

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibo15LKict5OvCibo6dCOmZicFmSbAia5yLRvC6mbXQKV0TjNFtUQkMDyrWg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

将他原有的index.asp改为webshell的网页



4.打开webshell

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib5qicBHEdW4ynDpUP2aIEKvGZKhIcjiaibZb7LBUmC1XW2ZWdMks6yRTZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



首先去GitHub上去找一个webshell后门

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibDUX7JhdvNk35rqTbojGdXYIlltn9j2XGBMvcgEakVLY5TyicVfmdHUw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

然后写入我们自己的后门



- 
- 
- 

```
$password='admin';$url=$SERVER['HTTP_HOST'].$SERVER['PHP_SELF'];echo "";
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibLUZWmtaLABWlpxzf7ugrRmGKl1Td39qotcDPRXZX6YIciaMgI98D6pA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



这时我们就打开webshell箱子发现我们的后门了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibRcqAkxKPL5wzmtCBWooJceMfzksn1rJLWZaf8IKbrYGspbMCj88nnQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**BEEF****安装和使用**

新版本的没有自带beef所以我们就要去下载

**1.****首先输入以下命令**

apt-get install beef-xss

执行完以后会发现有部分软件包无法下载，我们只需要根据提示，输入更新命令即可

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibKF2Yibf6EZhubkiaJEibr748GsG3JNUgFXQQEIBFgZyiaiaKj9z50mibSCdQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**2.****根据提示输入以下命令**

apt-get update

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibyN99STsgv0bH7neqAzjzlRPopnVe4qeBomn057NUKWEL8qQ0XynAQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**3.****再次输入安装命令**

apt-get install beef-xss

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibiafic7gWJKfNcwFMdnktf1OplsISSP3WDdxUFWTzFjIQ1JUuZ6FvNibIg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**4.****安装****apt--fix--broken install**

apt --fix-broken install

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibgvpibTj6bv3S1UVHQZ4nYjmIYRwXdNFF6FZ06Gwc8bNyVLfsEoqTqOA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**5.****输入****beef-xss****启动**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibsZBrZQRUlFKlpCxeyibFfjpO3nXxadhfNk74GwYy9CkOgP7rbVoQfaA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**6.****如何攻击**

输入这个命令就可以进行攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibdOcJnz86Zt7NYf7W669EMfpichdTN5XwdHfPpC2vxzD6n6xoaCp4Gjg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在在线订单系统里面输入代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibCvSSqYPd8RDYjlunaZKy5ibu2qWSy21Licg3Xaib9v0QxJbfbwmXiaMy6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后就可以去beef控制界面进行查看了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibqicpQkaGdzrJib1RJmN3ufB6RIghAPDjpmpJz9ztm0zyibk3ibicnQN9IGA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后我们就可以进行攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibXZNpcmUfp7uhSTKrNFbDNN0iaPy61J0hQC9TYj4EKySah4d620beHjQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib0yeUrt1HbdZtPt33R5O9YlSrqGwhw591mplnKicBehIOQXjK2ibCfdYQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

**Day 27.xss****跨站之代码及****httponly****绕过**

**1.****什么是****HttpOnly?**

如果HTTP响应头中包含HttpOnly标志，只要浏览器支持HttpOnly标志，客户端脚本就无法访问cookie。因此，即使存在跨站点脚本（XSS）缺陷，且用户意外访问利用此漏洞的链接，浏览器也不会向第三方透露cookie。如果浏览器不支持HttpOnly并且网站尝试设置HttpOnly cookie，浏览器会忽略HttpOnly标志，从而创建一个传统的，脚本可访问的cookie。



**2.javaEE****的****API****是否支持****?**

目前sun公司还没有公布相关的API，但PHP、C#均有实现。搞javaEE的兄弟们比较郁闷了，

别急下文有变通实现



**3.HttpOnly****的设置样例**

javaee

- 
- 

```
response.setHeader("Set-Cookie", "cookiename=value; Path=/;Domain=domainvalue;Max-Age=seconds;HTTPOnly");
```

具体参数的含义再次不做阐述，设置完毕后通过js脚本是读不到该cookie的，但使用如下方式可以读取Cookie cookies[]=request.getCookies();



C#

- 
- 
- 

```
HttpCookie myCookie = new HttpCookie("myCookie"); myCookie.HttpOnly = true;Response.AppendCookie(myCookie);
```



VB.NET

- 
- 
- 

```
Dim myCookie As HttpCookie = new HttpCookie("myCookie") myCookie.HttpOnly = True Response.AppendCookie(myCookie)
```

但是在 .NET 1.1 ,中您需要手动添加

- 

```
Response.Cookies[cookie].Path += ";HTTPOnly";
```

PHP4

- 

```
header("Set-Cookie: hidden=value; httpOnly");
```



PHP5

- 

```
setcookie("abc", "test", NULL, NULL, NULL, NULL, TRUE)
```

最后一个参数为HttpOnly属性



参考资料：

- 
- 
- 

```
http://www.owasp.org/index.php/HTTPOnly转自：http://yzd.iteye.com/blog/787190http://www.oschina.net/question/100267_65116
```

  将cookie设置成HttpOnly是为了防止XSS攻击，窃取cookie内容，这样就增加了cookie的安全性，即便是这样，也不要将重要信息存入cookie。

如何在Java中设置cookie是HttpOnly呢？

  Servlet 2.5 API 不支持 cookie设置HttpOnly http://docs.oracle.com/cd/E17802_01/products/products/servlet/2.5/docs/servlet-2_5-mr2/

建议升级Tomcat7.0，它已经实现了Servlet3.0http://tomcat.apache.org/tomcat-7.0-doc/servletapi/javax/servlet/http/Cookie.html但是苦逼的是现实是，老板是不会让你升级的。



那就介绍另外一种办法：

利用HttpResponse的addHeader方法，设置Set-Cookie的值cookie字符串的格式：key=value; Expires=date; Path=path; Domain=domain; Secure; HttpOnly

- 
- 
- 
- 
- 
- 
- 

```
//设置cookieresponse.addHeader("Set-Cookie", "uid=112; Path=/; HttpOnly");//设置多个cookieresponse.addHeader("Set-Cookie", "uid=112; Path=/; HttpOnly");response.addHeader("Set-Cookie", "timeout=30; Path=/test; HttpOnly");//设置https的cookieresponse.addHeader("Set-Cookie", "uid=112; Path=/; Secure; HttpOnly")
```

在实际使用中，我们可以使FireCookie查看我们设置的Cookie 是否是HttpOnly



**4****、使用****HttpOnly****减轻最常见的****[XSS****攻击****]**

  根据微软Secure Windows Initiative小组的高级安全项目经理Michael Howard的说法，大多数XSS攻击的目的都是盗窃cookie。服务端可以通过在它创建的cookie上设置HttpOnly标志来缓解这个问题，指出不应在客户端上访问cookie。客户端脚本代码尝试读取包含HttpOnly标志的cookie，如果浏览器支持HttpOnly，则返回一个空字符串作为结果。这样能够阻止恶意代码（通常是XSS攻击）将cookie数据发到攻击者网站。

**5****、用好****[Web****应用防火墙****]**

  如果代码更改不可行或成本太高，可以使用Web应用程序防火墙将HttpOnly添加到会话cookieMod_security - using SecRule and Header directivesESAPI WAF - using add-http-only-flag directive支持HttpOnly的主流浏览器有哪些呢？谷歌了一下，常见的浏览器都支持。

**httponly****绕过**

  可以直接拿账号密码，cookie登录. 浏览器未保存读取密码:需要xss产生于登录地址，利用表单劫持浏览器保存账号面：产生在后台的XSS，例如存储型XSS



**xss****练习靶场**

**第一关**

我们发现name可以控制显示那么我们就在后面插入xss语句来判断是否有漏洞

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibibybRicpm5m2dmWQGxMMOYlSOXdibwY5icvukhMOTcagWhh8zPxhpjrsRQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibWnmE1TWHxjGxb849tbhrgOL5xCv8hxofQpO0FG0uz3sXwCOwhibULhA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

发现可以，直接通过第一关



**第二关**

输入代码后发现并没有执行弹窗，然后我们查看源代码发现代码被转义了



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibd5wPckpaWb5Ij32aHWu5S832dFBELGuTYE9ibXSyUVE88ic5QFuzPKFg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这里我们就需要把前面的<这个符号还有“给闭合掉就行



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibsStZ99nAK2B9UUqSjjFEIee6LRU6dTvdhRVWJ461XmYAVtmtMPAI4A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

把符号转换为实体化标签，xss经常过滤的情况



**第三关**

这里输入后还是没有啥反应我们就继续查看源码，

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibVNqZnib7ibkaWrVibETiavFzibAq8JBu7hummkAIt7lqFria73ficcszOeQPw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现我们的<>这个符号被转译了，利用表单的鼠标点击属性。 'onclick='alert(1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibIh2aZ7TWEIUACgt0jRKXkbfgvAUOaep3019HfD7wyQQ8mgqnohXVHA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibUJQicATjXGLgrnZGr4UzTZ53lAjdReZsJniaBVjhqWhbvyVUblNE8Mqg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**第四关**

我们输入代码发现这关讲我们的<>这个符号进行了删除

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibGm6ib1MtUIibNceJ7OEgDFeMGXtFz8hyb0rALXBY7GLCLkhYsrK2iczuQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



再输入表单的鼠标单击属性来测试“onclick=”alert(1)我们发现和第三关是一样的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibI2mMwZFQL1mhET9JmHfQHS1zwLazllbwXWeKCaZFpHOWsgjqDLdEKg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**第五关**

这一关查看源码发现和第二关是一样的，但是既然存在那么我们就用另外的方法去进行绕过。借助aherf属性，自己创建一个javascript代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib5B1xaibJSCBaQk1grhDbibvxRT80cWsEdqbsUQeo29FdZC06yYpTohVg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib8o5xmlthI3QXjNlYNXPicTUaZAiaic8E2eicacMhibVdic9VJnaINOYUkGag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**第六关**

继续用第五关的代码，发现herf被过滤，查看源代码。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibBVp7hEsSzNbvj78hcLvzQ1aMZe30wHEuWezxKc06ykaxdhLxyMic5bw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现被过滤了我们试试用大小写去绕过一下

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibjEgquJLViaqiavXoW7H1g8hhwMia5gZ7YaTTDCpSaFyA9a5LN7gAqG6Rg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**第七关**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibaicW7zahvjC6U9mic4icluCNTBd79TianoaViaUVO1fyQ5ib3BM3dib6UOGsA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibv2P5mhHIuwu2Ht0LbE6MXZB3ISf18raIMzsWclCsOic0TwZMP2OXjWQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

和upload-labs一样，代码并无循环过滤，因此可以双写绕过



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4iby75xH5Ro4MrP0dUgxuy6JoOh7aRuQGSpVsgClIYbvEsCRhWrd9T3LA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**第八关**

大小写，双写均不行，替换为unicode编码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibEHU4a6JibsmrARVPibA58ZWYyGCrcN56dje3o7wokn0dmAiahicQJ8QiarA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib5R6zQjyZsa3NQq0r1oo782KEFdFw0NIHbUDfuVrq2UeGKsGAiaGzr3Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**第九关**

这一关不看代码几乎很难完成，代码会检测是否存在http://

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibgeWmYibIN6bMODxOym2iczHvBSoaXdBiaz2X05ibBgXPicia0EFadZQz1L1A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibTu4T8XnC76ylQ7OX7VI6VkrFyTBFcnkLwibMWXn3oWYm6VpWzJRmexA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



用前面那一关的编码会发现他提示连接不合法，我们在后面加入//http://后再去编码，就能通过了。



**第十关**

这一关啥也没有我们就直接看源代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib8FFu2vKoibZybKWicz7POjbOhicYGibRH2nCzWiaaNnwaUFEL87sk7Hl5Og/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



这里有3个隐藏的表单在源码中有一个隐藏的表单。其中含有 t_link t_history t_sort 这样三个隐藏的 <input> 标签先测试一下

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibH0tia6tdwibGINJOT2dhv2nibLS06hoOUFIw3hPonsGhJPulDLLrFHXrQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibLnlP9juFXUq5rHBvzzkrUuF9FArvA2HTmeJrKsvRg0T8WMKRMqLwTQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这里有三个 <input> 标签的话，也就意味着是三个参数



看看哪一个标签能够被突破

构造语句：?keyword=&t_link=" type="text"&t_history=" type="text"&t_sort=" type="text"

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibKfRNLJMwuX6UBjd0kdg4aQkfsPajtRAXu9eHmWyWT3dx5ribDHvFr6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



从页面响应来看，有一个 <input> 标签的状态可以被改变。这个标签就是名为 t_sort 的 <input> 标签，之前都是隐藏状态，但是通过构造参数响应发现只有它里面的值被改变了。因此可以从该标签进行突破，尝试能不能注入恶意代码进行弹窗。

构造如下代码：?keyword=&t_sort=" type="text" onclick="alert('xss')

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibbR5Mrw9ibD1W5FyZtOaTv2a8KPhHjnNlXOp7AfhNTIXIXXEj21mAMXA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



1：说明是接收 t_sort 参数值的。

2：会删除 t_sort 参数值中的 < 和 > 。从这里来看的话这一关就只能是将js代码插入到 <input> 标签的属性值中来执行而不能通过闭合 <input> 标签引入新的标签来触发xss了。



**第十一关**

​    这一关和上一关差不多直接看源代码，可以看到如同第十关一样有隐藏的表单不同的是多了一个名为t_ref 的 <input> 标签。尝试用上一关的方法看看能不能从这几个标签进行突破注入代码。构造代码然后构建语句?keyword=goodjob!&t_link="type="text&t_history="type="text&t_sort="type="text&t_ref="type="text

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib1iauLD0MQmS69QTRp9gToMVY5VtQfPamdG4DhOicqicTmicoA1FQsQRPkA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



  页面没有反应看看网页源码原来t_sort仍然是接受参数值的，但是里面的双引号被编码了这样浏览器只能正常显示字符但是却无法起到闭合的作用了。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib3Fx18aib5MEKFufga33QjPEHh7iaOicVEp2dovc6xXBTjUoZzjEq3do7Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  这几个属性其中$_SERVER['HTTP_REFERER']这个是检测来源的一个语句，value是接收的.$str11的值这时我们就抓一下数据包在数据包里面加一个属性叫referer

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib0JWe6MYZ1xc4qCfRgqDnJ1PH6cObjpmmtyAjKs3epZBwsLBK3jEkyg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

然后这里的value就有值了这个时候我们就写一个语句进去referer: " type="text" onclick="alert('1')"

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibHZJhlxEXPH22LbovibDTkKaD8qtLmWXSWL6v2xWAMhDCEsC5VdE7PXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



这样发送出去这时我们就过关了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibYY11icDYjCn8d53MwDhQBCxQ0tuHOsTpSEeic6gxqtPe4iagkjx6O9ntQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibDbHN9I0nzLpdKGzPrgvq0gplKicgSZGnEyBCapNA39uLQ6Nl7kq09rQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

**Day 28.xss****跨站之****waf****绕过及安全修复**

***\*常规\**\**WAF\**\**绕过思路\**\**WAF\**\**分类\****

- 0x01 云waf在配置云waf时（通常是CDN包含的waf），DNS需要解析到CDN的ip上去，在请求uri时，数据包就会先经过云waf进行检测，如果通过再将数据包流给主机。
- 0x02 主机防护软件在主机上预先安装了这种防护软件，可用于扫描和保护主机（废话），和监听web端口的流量是否有恶意的，所以这种从功能上讲较为全面。这里再插一嘴，mod_security、ngx-lua-waf这类开源waf虽然看起来不错，但是有个弱点就是升级的成本会高一些。
- 0x03 硬件ips/ids防护、硬件waf使用专门硬件防护设备的方式，当向主机请求时，会先将流量经过此设备进行流量清洗和拦截，如果通过再将数据包流给主机

**
**

**WAF****身份认证阶段的绕过**

  WAF有一个白名单，在白名单内的客户请求将不做检测



**0x01** **伪造搜索引擎**

  早些版本的安全狗是有这个漏洞的，就是把User-Agent修改为搜索引擎，便可以绕过，进行sql注入等攻击，这里推荐一个谷歌插件，可以修改User-Agent，叫User-Agent Switcher

**0x02** **伪造白名单特殊目录**

  360webscan脚本存在这个问题，就是判断是否为admin dede install等目录，如果是则不做拦截,比如GET /pen/news.php?id=1 union select user,password from mysql.user可以改为GET /pen/news.php/admin?id=1 union select user,password from mysql.user或者GET /pen/admin/..\news.php?id=1 union select user,password from mysql.user

**0x03** **直接攻击源站**

  这个方法可以用于安全宝、加速乐等云WAF，云WAF的原理通过DNS解析到云WAF，访问网站的流量要经过指定的DNS服务器解析，然后进入WAF节点进行过滤，最后访问原始服务器，如果我们能通过一些手段（比如c段、社工）找到原始的服务器地址，便可以绕过。



**WAF****数据包解析阶段的绕过**

  0x01 编码绕过最常见的方法之一，可以进行urlencode。

  0x02 修改请求方式绕过大家都知道cookie中转注入，最典型的修改请求方式绕过，很多的asp，aspx网站都存在这个问题，有时候WAF对GET进行了过滤，但是Cookie甚至POST参数却没有检测。还有就是参数污染，典型例子就是multipart请求绕过，在POST请求中添加一个上传文件，绕过了绝大多数WAF。

  0x03 复参数绕过例如一个请求是这样的GET /pen/news.PHP?id=1 union select user,password from MySQL.user可以修改为GET /pen/news.php?id=1&id=union&id=select&id=user,password&id=from%20mysql.user很多WAF都可以这样绕



**WAF****触发规则的绕过**

WAF在这里主要是针对一些特殊的关键词或者用法进行检测。绕过方法很多，也是最有效的。

  0x01 特殊字符替换空格用一些特殊字符代替空格，比如在mysql中%0a是换行，可以代替空格，这个方法也可以部分绕过最新版本的安全狗，在sqlserver中可以用/**/代替空格

  0x02 特殊字符拼接把特殊字符拼接起来绕过WAF的检测，比如在Mysql中，可以利用注释/**/来绕过，在mssql中，函数里面可以用+来拼接,例如GET /pen/news.php?id=1;exec(master..xp_cmdshell 'net user')可以改为GET /pen/news.php?id=1; exec('maste'+'r..xp'+'_cmdshell'+'"net user"')

  0x03 注释包含关键字在mysql中，可以利用/!/包含关键词进行绕过，在mysql中这个不是注释，而是取消注释的内容。例如,GET /pen/news.php?id=1 union select user,password from mysql.user可以改为GET /pen/news.php?id=1 /!union/ /!select/ user,password /!from/ mysql.user



**XSS****绕过****WAF****方法总结**

**1****、****script****标签**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibCkaf71Z4XgBbS68jGBd2h3jTGEvurTyAicWvClZZI6PsYQ7nqooAWuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**1.2 JavaScript** **事件**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibm98xxxVAFpdS0MfdxJtnXBzLjH9dqjnCw3maPuAEYzRsPpAXpJCFicg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**1.3** **行内样式****(Inlinestyle)**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibu7kXWic6rEhia5fGIEa3BKVmCkZ6icNS9xftEXEX7Ev3Q0iah7lOqkh4Ow/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**1.4 CSS import**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibyIGibh4vIyKb5EqDVhYSke2vRaahATzHhbYl033eyuekSKedianCeGrg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**1.5 Javascript URL**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibOebkh1ibupUhATicV2NYicqIV93Vjl0gTO3TwNnspogRw57u4NlDqVLtw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**1.6** **利用字符编码**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibMvIqUCCOldldrlVYNtuhHuWezib1IW4B38eULXDkcEIASR2tOhVsLNw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**1.7** **绕过长度限制**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibOsLXViaj6lN826LkKTiciceQicdmqk1ERdT5uc52ObNicOJPseFx7SibIliaA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**1.8** **使用标签**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibrw6MCDLRPlYubIa5HoGrEGenQEI1QU9o52IicfceXme5Srq4icbIysAQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibdrXvsqjteLFywwYybrAk6Q7nRHZIlOMzLvdhydZqYTIeCkh0NAqLkw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.2.1** **如果大小写不行的话，被过滤**

尝试<script>alert(1)</script>；

**
**

**2.2.2****使用****标签**

测试<a href=“ http://www.google.com">Clickme<a被过滤？href被过滤？其他内容被过滤？如果没有过滤尝试使用Clickme尝试使用错误的事件查看过滤<a href=" rhainfosec.com"onclimbatree =alert(1)>ClickHereHTML5拥有150个事件处理函数，可以多尝试其他函数<body/οnhashchange=alert(1)>clickit

**2.3** **测试其他标签**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibUQkYU9iaDtcy56MYIAq5TaS3PtbF3kkk4dfHNsl4VfWl1xNSIU4NhLw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibORfDuZyC1zSTJZKMYrC6G3yIRwYpsjDxMnNcBOq6l9DSk9AqhwkMGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibkZVm2Q80KgTuVu4wIfoo3jstUlONhpTTw60yHpUPe2pnnS7TKlgUpw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib2JTp9oYSIpuAt2bEpaDN2HvSmQAqg3QhHwI1OCsO7icfFZicVwaDwFcw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibibs3eGsRd52WB8nnHMy7zFFB6Y8qjgRIG49nKROoITIKo2iczwYyHsCQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.4** **基于上下文的过滤**

  WAF最大的问题是不能理解内容，使用黑名单可以阻挡独立的js脚本，但仍不能对xss提供足够的保护，如果一个反射型的XSS是下面这种形式



**2.4.1** **输入反射属性**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibW4dXicy7J0j2Mgtp6kYZzd05lcQZl8k5RaDNl4WJBicicTBXuBVhvXJGQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib3Hc6MXiat2EvLjJtIKrcFdt3BwsF1AZibvtiaOGH6I1UrRjiaeibSotyWOA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**2.4.4** **变形**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibqdr1TtDEqv4Sic0GQ3cDrtkuBuavj03TNQBrEAM6ZCwjNVeAuhoetrA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**2.4.6** **输入反射在****svg****标签内**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibE0qsozjrXWT3RxOljNiauWWPgbWsA66G5WKicMiaVpDsqnTRUJUgic4FXQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.4.7** **字符集****BUG**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibzo84D2ibcEEO4GkNIxFiaVrT60iaib6icyfc3ZPrPgorWfHMMatKKNqYTQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.4.8** **空字节**

最长用来绕过mod_security防火墙，形式如下：<scri%00pt>alert(1);</scri%00pt><scri\x00pt>alert(1);</scri%00pt><s%00c%00r%00%00ip%00t>confirm(0);</s%00c%00r%00%00ip%00t>空字节只适用于PHP 5.3.8以上的版本



**2.4.9** **语法****BUG**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibghI7JbibDDSn7CGAzAuIqLNPzsU8NIZvPye3UVVNXwrcHIfCHdWic1sg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.4.10Unicode****分隔符**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibM9c8P5kjibUXH0jOcEM1wdRuh8KZD2DonRYrNDmaZfne7Sen3fS6FnQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.4.11****缺少****X-frame****选项**



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibpqnq1jvLABbno3iam6aEwed3roZibXkrDULiamKzd4nT1odxkiaoI3AKuA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.4.12Window.name****欺骗**



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibuictq0S9TStia9fLDVyWCkmUkhiaEUddqdKAGhB3RYeBj62PXGGpibdzZA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**
**

***\*2.4.13ModSecurity\**\**绕过\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibS66XYVghTjvicNF26fggbzJfib37USWlbVgzSfttsEF6DVS8Pscd8V6A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2.4.14WEB KNIGHT****绕过**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibGGyQyicRumdiaRClaSkTh9FibzPS57rtPB8l8kS8RLzcHCwszDKgkIXiag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**安全修复方案**

开启httponly，输入过滤，输出过滤等

php：http://www.zuimoge.com/212.html

JAVA:http://www.cnblongs.com/baixiansheng/p/9001522.html



**自动化****xss****绕过****waf****测试演示**

**XSStrike**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibuSWhh75p7P4ceF1E1sGOib9MPh203iaZFpvzkdBr6veX2FxdqZJCg4Tg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**下载安装**

下载地址：https://github.com/s0md3v/XSStrike

最新版支持python3windows、linux系统都可以运行完成下载之后，

进入XSStrike目录：

- 
- 

```
cd XSStrikepip install -r requirements.txt
```

**使用方法**

1.测试一个使用GET方法的网页：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibZTkqUmOltboLSaMkTxVFXw0qM1efNxywCBscQUE7TAMzjG9XoUAibwg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2.测试POST数据：



- 
- 
- 
- 

```
python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- data "q=query" python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- data '{"q":"query"} --json'
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibonIic9zqnVUstqge425GoRQ8A6EDLN7vQU5s1QggbuSuibOtiaqWH7ziag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



3.测试URL路径：

- 

```
python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- path
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibUiaWq5o3KUHQQXuVMOzq1yaDYV4YKgn6BaLw5dpQibiaFTEib35bibYP1Ig/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



4.从目标网页开始搜寻目标并进行测试

- 

```
python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- crawl
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibWOGYxmx0yickDtFtib5asJaQRKW9zMBMicSPmHD7claRgAFTZUwRUyK4A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



您可以指定爬网的深度,默认2：-l

python3 xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" --crawl -l 35.如果要测试文件中的URL，或者只是想添加种子进行爬网，则可以使用该 --seeds 选项：

- 

```
python xsstrike.py --seeds urls.txt
```

6.查找隐藏的参数：

通过解析HTML和暴力破解来查找隐藏的参数

- 

```
python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- params
```



7.盲XSS：爬行中使用此参数可向每个html表单里面的每个变量插入xss代码

- 

```
python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- crawl --blind
```



8.模糊测试--fuzzer该模糊器旨在测试过滤器和Web应用程序防火墙，可使用 -d 选项将延迟设置为1秒。

- 

```
python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- fuzzer
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ib0l4fHd5uZ9gRD7XEBnFib53nF09Uh5puehng9WpPCVDURvZJnHjyUgQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



9.跳过DOM扫描在爬网时可跳过DOM XSS扫描，以节省时间

- 

```
python xsstrike.py -u "http://192.168.195.128/xss-labs/level1.php?name=test" -- skip-dom
```



10.更新：如果跟上--updata选项，XSStrike将检查更新。如果有更新的版本可用，XSStrike将下载更新并将其合并到当前目录中，而不会覆盖其他文件。

- 

```
python3 xsstrike.py --update
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibWLxclCkxZ60WLjYEnibsWByFsib3MSo1WiaQVeOs2Mge5loJbcdGLAl1w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicKheicW1rMB4bP1sK2pfib4ibpRzyF6ADjRaltKRyWVdKoCOsFPwSicsZQLXlkNChvRfjA7XCko3QN4g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)