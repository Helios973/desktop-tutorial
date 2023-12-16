# XD安全 学习笔记 | 应急响应

原创 特嘟 [0x00实验室](javascript:void(0);) *2021-09-10 14:11*

收录于合集#小迪安全笔记20个



声明

作者：团队成员-特嘟 【无名安全团队】 如需转载本实验室的文章，请标明来源即可。文章仅学习安全技术使用，切记请勿做它用，产生的后果与本公众号无关。



本文是Day73-75天的学习笔记。

------

## 一、应急响应过程

目的：分析攻击时间、攻击操作、攻击结果、安全修复等并给出合理的解决方案 保护阶段：直接断网，保护现场，看是否能够恢复数据 分析阶段：对入侵过程进行分析，常见方法为指纹库搜索、日志时间分析、后门追查分析、漏洞检查分析等 复现阶段：还原攻击过程，模拟攻击者入侵思路，关注攻击者在系统中应用的漏洞、手法 修复阶段：分析原因后，修补相关系统、应用漏洞，如果存在后门或弱口令，及时清除并整改 建议阶段：对攻击者利用的漏洞进行修补，加强系统安全同时提高安全意识

## 二、必备知识点

1、熟悉常用的web安全攻击技术 2、熟悉相关日志启用以及存储查看等 3、熟悉日志中记录数据分类和分析等

## 三、准备工作

1、收集目标服务器信息 2、部署相关分析软件和平台等 3、整理相关安全渗透测试工具指纹库 4、针对异常表现第一时间触发思路

从受害方提供的信息预估入侵面以及权限面进行排查，分为有明确信息和无明确信息两种情况：1、如果有明确信息的情况下，基本上会提出关于时间、操作以及指纹这一类的相关信息

- 基于时间：如果受害方提供了文件被修改日期、异常登录日期，那么我们就可以锁定这一时期的相关日志进行查看，不必去大海捞针一天天地看日志了。从而有针对性地对目标攻击事件进行分析。
- 基于操作：如果受害方提供了被删除、被加密的数据、文件位置，如数据库、磁盘等，那么我们可以根据攻击者的操作判断它入侵了哪些地方并可能分析出攻击过程。
- 基于指纹：如果受害方只说是网页被修改、网站被上马，那么我们可以根据攻击工具的指纹、木马的指纹、病毒的指纹、修改的内容等判断攻击者使用了何种工具、处于何种技术水平。

2、如果无明确信息的情况下，那么就需要排查全部可能入侵的手法：

- web漏洞：检查源码类别和漏洞情况
- 中间件漏洞：检查对应版本和漏洞情况
- 第三方应用漏洞：检查是否存在漏洞应用
- 操作系统漏洞：检查是否存在系统漏洞
- 其他安全问题：检查相关用户口令以及后门扫描

## 四、演示案例

### （一）windows+IIS+SQL日志搜索

1、查询IIS日志文件存放位置

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhj82B99fib53JkqpP5fntsB52I6ln8A8KxAfg2ueWPJTX1E6TPHRUIvQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

2、根据网站的ID号寻找对应的日志

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhBNVnlpHn0aicibhg0dkNjqFibPYQMexXnVrpncLFc1gBJdibT9bibefQewA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMh16iaApib8jV8vibwykEZnrTDHPjoSTicssuRAcWHYXSj9e2lz6YRpq4sRA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

3、找到日志后查看攻击语句

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhZ5aLDP3nltE6IMVjyBReib2AU7rGEdndndHYWJxPpRJICgdMDiaH6yYQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

4、根据指纹判断攻击所采用的了何种攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhjVVQ3Xdia94vl5PicqFyLf1QBY4nibMOgstGXWu4rNI6Ee6F2AJk8UA5Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### （二）linux+BT+Nginx+tp5日志后门 

1、在宝塔面板中下载日志至本地

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhnZDkxEko6GfZBDR3aATkF1LQBRE1mdo6v9DmN0H5coayFwiaFw83GIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

2、分析日志，查看工具指纹和攻击请求IP，锁定攻击IP

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhIsQp6pibBRff7pohSOpm2q2lysZ4RBGKiatF7eOSPJC1R3pgkGDdVBaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

3、宝塔自带后门查杀

### （三）简单分析日志

360星图

- 只支持apache、iis、nginx
- 需配置conf

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMh2rSv1w6zxXQDmWyKia9xHwmIk1Z8hjSoZuguPmS4Ct8U4sicTf0G7f6w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhegvJTSVma6RkZAXIuv6SH5PWpGCluGeAF0Dic7bQk9qYgLZRSp9gXbg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

其他好用的工具还有ELK、Splunk、FileSeek。其中ELK和Splunk部署相对比较麻烦。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhjdGyOUMSY6fuoGdEEHdfjeUDbcOxQI55UDkz3cPO5J9Qaoaet8BqvQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

# 74 - 操作系统分析（病毒、后门）- 应急响应 

## 一、常见思路及分析

### （一）常见危害

暴力破解：针对系统有包括rdp、ssh、telnet等，针对服务有包括mysql、ftp等，一般可以通过超级弱口令工具、hydra进行爆破 漏洞利用：通过系统、服务的漏洞进行攻击，如永恒之蓝等 流量攻击：主要是对目标机器进行dos攻击，从而导致服务器瘫痪 木马控制：主要分为webshell和PC木马，webshell是存在于网站应用中的，而PC木马是进入系统进行植入的。目的是对系统进行持久控制 病毒感染：主要分挖矿病毒、蠕虫病毒、勒索病毒等，对目标文件或目录进行加密，用户需要支付酬金给黑客

### （二）常见分析

账户：账户异常、账户增加，看攻击者是否留有后门账户 端口：异常端口开放，看是否与外部地址的某个端口建立了连接 进程：异常进程加载，看是否存在异常进程执行（排除系统正常进程） 网络：网络连接异常，看是否对局域网内其他IP地址进行请求（横向）或自身网络异常 启动：异常程序开机自启动，看是否存在开机自启动的程序，排查是否为恶意程序 服务：异常服务添加、启动，看机器上是否存在异常服务（排除系统正常服务） 任务：异常定时任务执行，看机器上是否存在定时任务 文件：异常文件，看机器上是否存在异常文件，如后门、病毒、木马等

### （三）常见日志类别及存储

#### （1）windows

事件查看器》windows日志（包括应用程序、安全、Setup、系统、事件）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhAn8xvgsMwcRP7osQmGS3sfKZqsLOwjjTX1F12VufJN2bHiaC8qO3jibA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### （2）linux 

cd /var/log

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMh3z9qMibJQa4pd167SJ0mV3oYHvnS9BrnCJVNK2EmicHOXDaEk5p0uFWg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### （四）补充资料 

https://xz.aliyun.com/t/485 

https://www.secpulse.com/archives/114019.html https://docs.microsoft.com/en-us/sysinternals/

## 二、实战案例

### （一）攻击响应-暴力破解(RDP、SSH)

通过kali模拟攻击对rdp服务进行暴力破解

```
hydra -l mac -P /usr/share/wordlists/metasploit/password.lst rdp://172.16.54.42 -s 3389 -vV
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhe6vIK2wkCQ6ia160LFiaIYyXibbNicdPvatKf9k98FG64efN120KwqyAHw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### （1）手动查看RDP

在win系统中需开启审核策略（成功、失败），同时查看windows日志，关注事件归类、事件ID、事件状态等

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMheRzVNia4Z6sTXzmGJGI4BM8iaiciaxHUp1vHCqaNCjcAvB2iaNwoL2ianYibQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhRgcsUxaCHtJDiaBCJSByibzTicfiaCxWYuic1Z0Gz9tZaU0Ve2cMAqbSwDA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

可以发现爆破账户为mac，黑客主机名为kali

#### （2）工具查看RDP

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhWzJ1YcjteZFV53LdoElVmGia6BQBuZ92ibnU3JRgH1A6mCBSQOE3hJ9g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

对SSH进行爆破 1、统计了下日志，确认服务器遭受多少次暴力破解

```
grep -o "Failed password" /var/log/secure|uniq -c 
```

2、输出登录爆破的第一行和最后一行，确认爆破时间范围

```
grep "Failed password" /var/log/secure|head -1 grep "Failed password" /var/log/secure|tail -1 
```

3、进一步定位有哪些 IP 在爆破？

```
grep "Failed password" /var/log/secure|grep -E -o "(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[04][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][09]?)"|uniq -c | sort -nr 
```

4、爆破用户名字典都有哪些？

```
grep "Failed password" /var/log/secure|perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}'|uniq -c|sort nr 
```

5、登录成功的日期、用户名、IP

```
grep "Accepted " /var/log/secure | awk '{print $1,$2,$3,$9,$11}' grep "Accepted " /var/log/secure | awk '{print $11}' | sort | uniq -c | sort -nr | more
```

### （二）控制响应-后门木马（webshell、PC）

windows：默认配置测试 注：windows高版本网络信息获取不全（win2012及以上）

linux：借助 CrossC2 项目

```
netstat -ntulp 
```

https://github.com/gloxec/CrossC2 

https://github.com/darkr4y/geacon 

参考过程：http://www.adminxe.com/1287.html

1.项目上传至CS服务端目录，给予执行权限

2.配置监听器：监听器为windows/beacon_https/reverse_https 注：如果是阿里云记得端口放行，同时需要关闭linux默认防火墙

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhLyk1SqqGg43LfWQmYdImZ23vFogqY4BjhdGgMOnQI5TYib0LXsBt1Kw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

3.生成后门：

```
./genCrossC2.Linux 47.99.49.65 5566 null null Linux x64 C2 
```

通过网络监听工具及 windows 日志分析或执行记录查找后门问题

### （三）危害响应-病毒感染(勒索 WannaCry)-Windows

详细说明中毒表现及恢复指南

https://lesuobingdu.360.cn/ 

https://www.nomoreransom.org/zh/index.html

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhJjnzRF0KDNvXzkJMh4LicytwVJ9vicBMicCoLlicgvuHvr5eejviatmB0iaQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhickcDnwm1UuSwr0nztY0iaU8tFeUdqLicOwuusyWJ23zaO4PSTQhsNrdA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### （四）自动化响应检测 

Gscan 多重功能脚本测试-Linux

```
python3 GScan.py
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhqhEmcq9LIbaiaFfdZHRk5Orx3w2oq1Eysiap1waCH4FcZwlicM5xbCF4Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

## 三、涉及资源：

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
https://xz.aliyun.com/t/485 https://lesuobingdu.360.cn/ https://github.com/gloxec/CrossC2/ https://github.com/darkr4y/geacon/ https://github.com/grayddq/GScan/ https://bbs.pediy.com/thread-217586-1.htm https://www.nomoreransom.org/zh/index.html https://docs.microsoft.com/en-us/sysinternals/ https://www.secpulse.com/archives/114019.html 
#网盘链接 失效不补https://pan.baidu.com/s/1tQS1mUelmEh3I68AL7yXGg 提取码：xiao
```

# 75 - 第三方应用分析及应急取证 - 应急响应

## 一、必备知识点

1、第三方应用由于是选择性安装，如何做好信息收集和漏洞探针也是获取攻击者思路的重要操作，除去本身漏洞外，提前预知与口令相关的攻击也要进行筛选。2、排除第三方应用攻击行为，自查漏洞分析攻击者思路，人工配合工具脚本 3、由于工具和脚本更新迭代快、分类复杂，那么打造自己的工具箱迫在眉睫

## 二、案例分析

### （一）win日志分析神器 - LogonTracer

如果是阿里云主机需要开放端口并关闭防火墙 1、下载并解压neo4j 下载地址：https://github.com/JPCERTCC/LogonTracer

```
git clone https://github.com/neo4j/neo4j
tar -zvxf neo4j-community-4.2.1-unix.tar
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhcjgBT8DPxKXHOgBF84U8COL4DtCq4qQCP94W2iabibqD5fcHkrk5CFWg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

2、安装java11环境

```
sudo yum install java-11-openjdk -y
```

3、修改neo4j配置保证外部访问

```
dbms.connector.bolt.listen_address=0.0.0.0:7687
```

4、开启neo4j

```
./bin/neo4j console &
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhfiaUHBmSFvsqjHNHNC9TbUZM5VzZ8z4bDc1dNO9icRhzCw6SssOJgIXA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

截屏2021-08-22 上午8.39.54

5、下载Logontracer并安装库

```
git clone https://github.com/JPCERTCC/LogonTracer.git 
pip3 install -r requirements.txt
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhsvsAYeJ15wS0ppaodk0xgDMyMGDl1SYAoibAXYSJiakTPfxwUiaHO0M0Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

6、启动Logontracer并导入日志分析文件

```
python3 logontracer.py -r -o [PORT] -u [USERNAME] -p [PASSWORD] -s [IP 地址] 
python3 logontracer.py -r -o 8080 -u neo4j -p xiaodi -s 47.98.99.126 
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhTjJBicVeby6EpNbVhOqxZLVWhaLNPJMN7wKibK71c3PTCEUZsW7ILJLQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

截屏2021-08-22 上午8.42.20

```
python3 logontracer.py -e [EVTX 文件] -z [时区] -u [用户名] -p [密码] -s [IP 地址] 
python3 logontracer.py -e Security.evtx -z -13 -u neo4j -p xiaodi -s 127.0.0.1
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhiaeNd3OTYiad0O0fU2LkHszfiaGG2Xnzjxz8GByNIcvASzrRPvEBqiaS7g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

截屏2021-08-22 上午8.46.16

7、刷新访问LogonTracer-web_gui，查看分析结果

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhyF2B1Q0sibKExDEbkD6vgexibGvUPiaP5RkXz9fkeZafkbEkf14lWXyTA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhYEicvxSgtia4Fo7PpHkKh5PKE3tpRJqpQnNgLR9iaxP2M2OyeiaRvvtDqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### （二）数据库（Mysql、Msql、Oracle等）日志分析（爆破注入） 

常见的数据库攻击包括弱口令、SQL注入、提升权限、窃取备份等，对数据库日志进行分析，可以发现攻击行为，进一步还原攻击场景和溯源攻击源

#### （1）Mysql

1、日志启用并查看

```
show variables like "%gengeral";
SET GLOBAL general_log = 'On';
# 这里可以设置mysql日志存放目录
SET GLABAL general_log_file = '/var/lib/mysql/mysql.log' 
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhujHz9icuXG4NOniaF177uTS69uCYpwNgJOMVGeo4CAicAPmXxvxiaVexyw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

2、通过超级弱口令工具（snetcraker）对目标进行爆破模拟攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhPsibwVicNDeKCRBvNjtibanwibUSvCqAXblqtsibcJYVxkndib7a42z8euqQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

3、查看日志并分析

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhXw2pJzZat3OBuyDWLVR26GbKpfC74HRanibdyiaibXiacgLGGw4RnR5TYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

#### （2）Mssql 

1、查看日志

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMh5nde9p65JK83WiaZLADflJsoYGUAnph7ibYolhhhAXqDmdpDmMTIk3dg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

2、配置跟踪文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhbibhrnjMm3ic7iao3AUGycVkvYibqYu8ehGssD47BlVqr2m3D3k9MAZeaA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

3、对Mssql进行攻击并实时查看日志

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhXo1Vdhrlnjwj4FgOaDan58rTQgibGRGDxicfO2xBazOibf0ibFJ4r7P8uA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### （三）自查漏洞模拟渗透测试寻找攻击源（漏洞、口令检索） 

主要针对以下两种情况：

- 日志被删或没利用价值
- 没有思路进行分析且可以采用模拟渗透

#### （1）系统漏洞自查（win、lin）

主要工具为WindowsvulnScan、linux-exploit-suggester

1、windows自查

```
D:\Myproject\venv\Scripts\python.exe cve-check.py -C -f KB.json
```

如果出现报错，将KB.json切换为utf-8模式

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhbXAia7fSNUfn74sIksos1f5DCLTXva3De3Uuqxkib46zECXoYj5zM1aQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

2、linux自查

```
./linux-exploit-suggester.sh
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhEF1mluzSehmrVwwtorWzeH5dmv1vdDptClfllPGnib6u2xBITVOO9nw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

工具地址 https://github.com/chroblert/WindowsVulnScan https://github.com/mzet-/linux-exploit-suggester

#### （2）服务漏洞自查

1、Windows

```
Get-WmiObject -class Win32_Product
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhLjd1kyKHJpJx2vF5fgRKa6goWPPG85mlzkW2iaL9sRCnqSNMopCfjlg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

2、Linux 地址：https://github.com/rebootuser/LinEnum

```
./LinEnum.sh
```

3、根据检索出来的服务进行漏洞扫描 主要使用searchsploit，比如weblogic

```
searchsploit weblogic
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhTLd9dyJseL9UEsZITk4L9ZaJicCdgL5qYqEoeyK5c3pDTTibej87YOHw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

其中查询的漏洞分别为dos、local、remote、webapps

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhcqHPHMsHSqrYGDHbxReia9VWmGkpu03XUSjY8cGFn83EfMqfqLibVKfg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### （四）自动化工具ir-rescue应急响应工具箱 

地址：https://github.com/diogo-fernan/ir-rescue

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicy3sln0zHKTBrAacxk8bMhN9zeUehpFUSDySanYFH4W8ZjflT54ZGxT4J0KMRoaDOZjFshzUsO4w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

主要用于打造自己的应急工具箱