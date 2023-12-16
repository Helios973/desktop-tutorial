# XD安全 渗透测试笔记 | 红蓝对抗

原创 N1vRANA [0x00实验室](javascript:void(0);) *2021-08-31 08:50*

收录于合集#小迪安全笔记20个

声明

作者：团队成员-N1vRANA 【无名安全团队】

如需转载实验室文章，请标明来源即可！

文章仅学习安全技术使用，勿做其他用途。



本文是day80-82天的学习笔记。

- 
- 
- 
- 
- 
- 
- 
- 

```
tips：因为条件不允许所以没有搭建AWD平台实例 有兴趣的小伙伴可以在Github上自行下载搭建 
#相关链接https://github.com/vidar-team/Cardinalhttps://github.com/mo-xiaoxi/AWD_CTF_Platformhttps://github.com/D0g3-Lab/H1vehttps://github.com/zhl2008/awd-platform
```



**1.AWD****模式**

**AWD (Attack With Defense****，攻防兼备****)****模式。你需要在一场比赛里要扮演攻击方和防守方，攻者得****分，失守者会被扣分。也就是说，攻击别人的靶机可以获取** **Flag** **分数时，别人会被扣分，同时你也要保****护自己的主机不被别人得分，以防扣分。**



**规则简介**

1. **出题方会给每一支队伍部署同样环境的主机，主机有一台或者多台。**2. **拿到机器后每个队伍会有一定的加固时间或没有加固时间，这个视规则而定。**3. **每个服务、数据库、主机上都会可能存在** **flag** **字段，并且会定时刷新。通过攻击拿到** **flag** **后需要****提交到裁判机进行得分，一般会提供指定的提交接口。下一轮刷新后，如果还存在该漏洞，可以继****续利用漏洞获取** **flag** **进行得分。**4. **AWD****模式按轮次进行，****flag****会在一定时间内刷新，一般为五到十分钟。考题一般为****web****和****PWN****方****面，漏洞一般也是常见漏洞，一台服务器不止一个漏洞，有多个漏洞。一般不会太难，尽可能多找****到漏洞。**

**
**

***\*比赛流程\****

- 1.比赛形式一般为主办方会给一个普通权限的用户让你SSH连接上服务器。普通权限很多都不可更改 
- 2.连接上服务器后迅速进行备份和代码审计。此时需要分工进行，团队一般为三到四人。赛前一定要做好准备。 
- 3.当你发现一个漏洞可以进行攻击的时候，写好批量攻击脚本进行拿分，同时需要队友将自己这个漏洞修补。
- 4.看比赛形式，有的比赛删库比pick失败扣的分数相同或者更多，一般分为三种扣分状态。pick失败， 被攻击，pick失败且被攻击。所以如果pick失败和被攻击扣分相同的话可以考虑先删库。毕竟pick失败 和被攻击一起扣的分更多。 
- 5.比赛时一般不能使用工具或者WAF等进行防御（有的比赛可以） 所以要注意比赛规则



**重点**

- **1.****比赛时****web****题目一般为一个网站****cms** **所以平时要熟悉一些常见的****cms****以及利用漏洞**
- **2.****比赛漏洞一般多为简单的文件上传、文件包含、命令执行、反序列化等。**
- **3.****比赛尽可能多利用命令执行漏洞或木马（方便批量化拿****flag****）**
- **4.****溯源攻击 查看流量以及日志****（参加****ciscn****线下赛的时候流量文件自己保存在了根目录 在****weshark****等****抓包软件内打开就可以查看）**



***\*5.\**\**下载到网站源码后放入\**\**seay\**\**或者\**\**d\**\**盾等工具内扫描漏洞\**\**进行修补和利用\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJxukuBT2fdgZic8y2ZO7UY7eficTwdxWA6NYsOIVz5KGicBAnTXcXxegGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJHkmjBGPJJ2yqJ13Z3OibtskM1qdawTtia2BOz3TX0NP6dpeS9DQlCgMg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJmlP0V4I5wGogwCickdP3Tq78WbEdsCpEqC4ulnwQ6WpVkgyj5zS2EPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- 
- 
- 
- 

```
# 日志地址 /var/log/apache2/ /usr/local/apache2/logs /usr/nginx/logs/
```

**
**

**备份**

- 
- 
- 
- 

```
# 打包目录 tar -zcvf archive_name.tar.gz directory_to_compress # 解包 tar -zxvf archive_name.tar.gz
```



**常用命令**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJMe14vSxDGCII4rydkHpcCHfaibZcjgEiaxf3l6icvKERTKC9tgv27BMibA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJ0FLZMLD9icCu2uzsmjP2490GcZX8kx1BXWl7kskJONicg1ShjQBia040Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**脚本**

**批量提交获取****flag**



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJkdEBeghzlibUTRMibdD56WLcpmag3XRcaicdCPlRianapz4EkqpPbZAoBw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJUy0KFKHrMLKeGtHyuh3MNIlw7ake7sK3R78C7ZkkpXkwWSiaicaic40IA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJNcq2FpXibnOLfM0gm8FOMyG6Q7muaZNpVUV7Ra4qEaRSHVsGibFTy7og/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJRZUsiaiawDEDrjcpDnsM0cYj8Req6grZNPe3TkgVvEV7Tha6DichiaGZTQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJ5chVX9FViaVnBJl8C0aUMK2r8VTWdwfCaxNcoPctFW59FGMD3MYJpNg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJ3Mia3hq0G9rj6f63eiaeyk9oIgTfQJmpy1hPSHDk2QP0ohO6N7I5MNbA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJtZ9WYjMhIM43YPZzibjOApU8HClttpIghNwB0A7MRyJb7zWAmthziaJA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



直接进行命令读取 

  cmd=type flag.txt

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJSlTDc2RLGsUWSUR8OHiaSc9ftqFu7fhekwt5lic5So8iaIgtNiaVEthGSg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**不死马**

不死马传入后需要进行访问一次触发

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJic74GcdynIPXBp8jYnHeBWswXr9syPwD0pksh6TbSkbxA3sxLYv9sfg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



不死马原理

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJiblOo5uicXlYualIIS0q3ibcqq0L6QA1ZXhs0Bs2nb87hJdU2QClhboDw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJhsaYGnl49j9OBxvkkXYmv0wLrNgPdctWQmtgZ0X6IIr9qPVqKLWM5A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJSnicWeVpXDwK3iaBAGqibgEQdtyiamN0tt727boHbWF7ESj2RzKtoxn3Vw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



克制不死马

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJkjEaWHIU0qVicqGC3HywibvanFpdbxpAK7mIDvHyLpPJJ70sLE2IBCOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



写一个和不死马一样的代码（把文件执行代码修改）sleep时间更短 造成条件竞争

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJKJEAdfAqricIYicWgRLHicQDN37Y1garnvL8oj1zBnJpc4oZIGe2RfKNw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



杀进程

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJGeRb3QNyAt7AGaUK7lOlo1Gk9WNoYpBT6Y1gG2rdacGQia4Wfk5B8Vg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

tips：有些比赛时禁止使用不死马的，严重影响比赛体验。



**搅屎棍**

  顾名思义 恶心别人 核心就是发送大量垃圾数据包给别人 让别人无法正常观察流量捕捉payload 此招就是干扰对手利用他人的payload 给对手造成干扰。但是有些比赛可能也会禁止 回手掏就是利用他人的webshell和payload进行攻击拿分 。



发送大量垃圾数据包

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJPiaARPuZAdcQbWsXdxv5jZicj8kzT40D9r38GyiczcQnjhnCibib1XT8YxA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**文件监控****(****也有可能禁用****)**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJicVKvosjqVJfEAibZnXmouMEcTP5LG0UgcibdIfubHx7BzUADdnMz0PoQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJqHdMpWAGLNcwSCCQuKjRSfBt7709eNibs3ibMOXzs53PmVj9lTyPs8iaA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJmrK2phqw14TlDPFvQALjm4aej8fQ44D2WmNKCGwOuIZLhqL3cmP3zw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJ8Bcict1GrtG2OsyzFjSyssO8icOKRkl7qQ4fgIqqrZg5gofajXDT09Iw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJ4YNBbhXhBM0Orv5A3B7cUOm9XiauNWwYw7NK3cGDfhmk6iaHAKI3ZWrw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJdz60OhibgWRlvz7xkVlbtuXoVzBhLcrLIu1Subdy4Hdaeic8WxiaPvia2A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJm5fAPHkaB8vyS0k9ZXGY1xW3r4PML3D8fDhjMe6njq5JuAvZAlMoaA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJNLkiahGcThxyJElUNlkV2niaicNGT8IxIbPZOND97WX28WB7Qju8Nollg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJGsxwiaeoYFWpDjn3jWn7w7ZjmtHLFictPssb4AjtmJy036ZSIpCCzhUg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJdmFxZzqHHT80ib0POa8j5TnVva5heCH7BZJp70MicT6njOWpZDsOGCUw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用在监控的文件前加入require_once('waf.php');



**微型****waf****（可能被禁）**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJbNSeguFzWYgRVTxicFfHmTczXcLv4PNeVYzvSd3BqBaictlJlia696d3g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJZcGmh3JTicjZib5oDERdrkvW9oLYZaOAibqDHzLs5Ys423clicicl2uplFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJFusbc7olkIaeicO1yCRZKOuDLbac83RaVH2qPyTnwaPy68jJ1NzJwbw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





------

**2.****蓝队****att&ck**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJVIBPOFVrqMPyNn287ibGFPn8CDMYtQ3jwuPbibuttHtgcxI34LnmwaVA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



***\*1.ATT&CK\****

***\*ATT&CK\**\**详细介绍可以看这个视频\****

- 

```
https://www.zhihu.com/zvideo/1305977738013540352
```

我们需要掌握的有 

- 1.认识ATT&CK框架技术
- 2.认识对抗的蜜罐技术的本质 
- 3.掌握WAF产品部署以及应用 
- 4.掌握IDS在对抗中的部署应用 
- 5.掌握威胁情报平台对象报告分析 
- 6.书写报告的专业性



**2.****蜜罐技术**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJWiclx4VRjb6tSze80CPaId5pFZYwGqOSvm1qyqfVic9ibxEIDl7ar1Tug/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

https://hfish.io/#/  开源蜜罐平台



详细说明书

https://hfish.io/#/?id=hfish%e8%ae%be%e8%ae%a1%e7%90%86%e5%bf%b5

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJhdWFqcNvH5CXibabHJDSiaHbcia95iacuCgniaUXz9pvJDlicH1iaTmxeyFTw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

利用蜜罐技术我们可以更好的保护好安全信息以及对攻击者的溯源追。



**3.IDS**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJxuIgibv6Vibjfiasv9U8x5lPic9kI07x4eKGtHIGy0ibuyCnSXmoM0jovGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJ8iaQdx3FXaqeAt1Plwtx2eugJnxpzick3m5lorNAiahjo8g0aiblRZS8VA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

功能点：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJrxiaibUPoILrmkh0cY3hEg4ITfT4KPDfibQZCQrj51KBNWTSZweXI86Nw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



NIDS：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ88iaBaH3YWJflMxqJrwfyqJuubAib1Amiay9f2eEUWWlK9MmBiaW0pXLLicAkGibdVhNeNZVuUzY44zaQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



往期推荐



XD安全 学习笔记 | SRC漏洞挖掘经验

XD安全 学习笔记 | CTF夺旗

XD安全渗透 学习笔记 | CSRF-SSRF阶段

XD安全渗透 学习笔记 | XSS漏洞阶段

XD安全渗透测试课 学习笔记 |  内网渗透(四)

XD-小迪安全-渗透测试课程 附完整资料版

XD安全渗透测试课 学习笔记 | 内网渗透(三)

XD安全渗透测试课程 学习笔记 |  内网渗透(二)

XD安全渗透测试 学习笔记 |  内网渗透(一)



  点个关注不迷路。