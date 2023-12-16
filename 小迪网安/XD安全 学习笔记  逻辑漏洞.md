# XD安全 学习笔记 | 逻辑漏洞

原创 Flyable [0x00实验室](javascript:void(0);) *2021-09-03 08:08*

收录于合集#小迪安全笔记20个

声明

作者：团队成员-Flyable 【无名安全团队】 如需转载本实验室的文章，请标明来源即可。文章仅学习安全技术使用，切记请勿做它用，产生的后果与本公众号无关。



day33-36天 B站小迪课程的学习笔记。



------



越权分为：水平越权和垂直越权（作用更大），未授权访问

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMK1BNM84mFvklzdtn5WzJ1fNmR6y4MzME8E2KWfI9UyYkywbjI5TsjQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- 
- 
- 

```
水平越权：通过更换某个ID之类的身份标识，从而使A账号获取修改B账号数据；垂直越权：使用低权限身份的账号，发送高权限账号才能有的请求，获取其更高权限的操作；通过删除请求中的认证信息后重放该请求，依旧可以访问或者完成操作
```



**水平越权**

pikachu靶场中逻辑水平越权登录kobe相关信息，登录，查看个人信息时对其进行抓包，

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMDsvAzdGB8sK8TO0r3r5iaoOMcmmSZnFldH0Ap0ibKbgg2dib12NKnUNXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

查看个人信息时对其进行抓包，可以得到

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMLNd3EwUStQhQfITPZma0kJYtoO1Pw27DaetUpl9UUteWetYlIoHoQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



将username更改为lucy，便可得到lucy的相关信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqM9gNE8a6foCyF5qRbxQrk2quC9twyqicVNNH94HgxHoVCLRTIs5kxFFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**垂直越权**

在登录admin(较高一级的管理员)的情况下，对其操作进行抓包，在登录pikachu（低一级用户）时，就可以将较高一级的包调出来，更改cookiePHPSESSID=的值，可以条件：需要有admin的数据包

更改相关的user值，便可以对其进行添加

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMWicPSMkzTVWutgCd9wTuZ0gL4SibNRiag6x3EHWE8u1G7aPTibAFiaSYOWg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

条件：可以抓到admin的数据包

1、普通用户前端有操作界面可以抓取数据包

2、盲猜

3、通过网站源码本地搭建自己去获取



墨者靶场

抓两个数据包，看到有个包的card_id的值，我们考虑用burp suite的thunder功能对其进行爆破，排序相关的length对较大的值进行查看，或者查看图片源地址，可以看到马春生的相关caid_id账号，在该数据包中查看账号和密码（可能需要解密），登录，得到其flag;

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMnlwvMwCO0icoAoRHN4r5muO35LHHgZWjqQcGpbmky4JWFbQcKMC30mA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

原理：

1、前端问题，界面，判断用户等级之后可选显示，直接根据用户的usertype值判断级别信息。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqM1at56BYiacnv5Y9iafRtDiaIY6t1zX9WGTjc3UzG77qnRHrticUjWp05Ww/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



secscan-authcheck插件

**检测：**

小米范越权漏洞检测工具、burpsuite的anthz插件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMhodv49grFAdLQ6mFvreLhNAAJZqTgjMkESSd4qOBx9zZSb7OCcAbfw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**浏览器插件authcheck**

  登录应用功能安全问题Http和https协议抓取Http抓包，密码一般是明文传输，也有加密传输，确定其加密方式，进而对其及进行爆破，MD5加密结果一般都是32位对密码进行加密之后爆破。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqM6muoc5w513clT0cC8B4AnvRjkiaIHic2gQsiaPibNqwVTBKNVKl13QcWBg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



https抓包，密码一般是密钥传输cookie脆弱性，

代码审计，比如有的只需要验证cookie有值进行，抓包，给cookie赋一个值，便可成功绕过cookie验证

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMicHDHEibbGRwdU0LjOT4HO0bXicpwibcLaRpmSjREM3zAQNuSfwibWrtFWg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



1. 抓包，修改相关值（ID,price,num,statu等），数量可以改为-1

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMlqKRcagxhfXIibayWBicrNoWxCOjDOYLF9JFN5Qricct2AUoLlUpL5Meg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

或者是，先抓取订单编号为10元的数据包，然后购买加个1000元的东西时，将数据包得订单编号更改为10元的，然后只需支付10元

2、或者抓包修改相关商品的信息，id值和name值。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMqUE6uZicbaBcGASsxXKbRCnhWrMROtTK955VeoqFAjufxffpNHzx3HQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMZAoiaRZYMZUMFmhLxcUIicK0ZxzazlrlzLbu2sdIxibHVR3EVybfTgZrA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



3、修改支付状态

找回重置机制：客户端回显，response状态值，验证码爆破（验证码次数，时间），找回流程绕过等接口调用乱用：短信轰炸，来电轰炸



墨者靶场密码重置-验证码套用-靶场

- 

```
https://www.mozhe.cn/bug/detail/K2sxTTVYaWNncUE1cTdyNXIyTklHdz09bW96aGUmozhe
```



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMwcdGiaOAccA2LiaBd2vOZt7Mhy9BupF7jiaqc4E3yPWAhUMIfwj3FapKg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

正常情况下，手机验证是第一个页面输入手机号，验证码，第二个页面重置密码，而该题目中，手机验证码和重置密码在同一个地方，步骤：先用要重置密码的手机获取验证码（验证码得不到，不用管），

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMJOQBaHxF6rw6qvGUkYMQ2bDQEIPBlNVXzWe3aHpvFeWKicdiaSvZVljQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后用已经注册的且在身边的手机号获得验证码输入，

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMlhAUtJWzgNf6PwLQ84fGyJAkicvcXiaQXiaj1u7kUyicibWSRZvNrhaicPEg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后抓包，将之前那个号码换成要重置密码的手机号

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMialo32Y9NVhHJ7GgSLLX47ybKAgBZgwibe2SAHSzH25cOnuyHrKw3xNg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

成功获得flag

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqM95xM1y3rt5w6LHOE1bzo1vSZoALSrGYT2N8qiaYZxZ3xcicVYzuVmKVQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

手机邮箱验证码逻辑-客户端回显-实例抓包，在数据包里面，直接可以看到验证码随便输一个验证码，抓包，之后全程接管，将状态码进行更改，例如将3改为1，以当前的回复值修改就有意义，若是以服务器来验证的话，就不行

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMgIric3HwCqI9heAFKGQLjfGnW2P6M131nv8kcoFplonibaP1MFJ8RVSw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

哈哈哈哈，迪哥的手机号

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMCTxCBYcMIGoOuYG78Az0IwoBicSw0hB81Osu3K3mBeJHXYianCeLLv0Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

验证安全：

1、Token(爆破、回显、固定)， 回显在浏览器前端可以看见，固定就是可以多次重复使用2、验证码（爆破、识别、复用、回显、绕过）Token是服务端生成的一串字符串，以作客户端进行请求的一个令牌，当第一次登录后，服务器生成一个Token便将此Token返回给客户端，以后客户端只需带上这个Token前来请求数据即可，无需再次带上用户名和密码



可以把页面回显中的Token值替换爆破中的Token值就可以绕过Token

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMElf3x8uCWWgZw8RtNzdd4fDIE3WqFm8FYdqA8QUSPWia3dbHB8rHr3A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMZKoPnHKfibErz3jLXXtLgLugTHvIwVDZkhjQZib6qMzVPAUFsDRN92VA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

- 

```
https://manage.yyxueche.com//panel/login.php
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMRC1kJ4VibwJKr5zX8yNaQeD1cNCFZicFHCZjPbtDtXM9XyibuEtn1czuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



复制图片地址

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMpyxKheGqJxlyqbYTibWibGC08fwfppAJjyC7CHhIClv9YgRptocbssOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



验证码地址，为验证码图片的链接地址

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMHbTolp9hFdiatcguSe0oicFcE0Ff37xxwny6YvnuQ7NuAHG9rA6Or5kQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMuV2ibZbLCz3drKBxgGEIIrbzSZAqx9fhpiaK2nJZBtRO7G2wQCUIBtew/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**爆破防范措施：**

1. 如验证码输入3次就重新获取
2. 每输入一下变换一次
3. 使用特殊字符集
4. 特殊的验证码如图形，滑动，判断图形等



**使用burp插件captcha-killer识别图片验证码**

漏洞产生的原因

通常情况下，一个 Web 程序功能流程是登录 - 提交请求 - 验证权限 - 数据库查询 - 返回结果。如果验证权限不足，便会导致越权。常见的程序都会认为通过登录后即可验证用户的身份，从而不会做下一步验证，最后导致越权。



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
隐藏URL直接对象引用多阶段功能静态文件平台配置错误修复方案前后端同时对用户输入信息进行校验，双重验证机制调用功能前验证用户是否有权限调用相关功能执行关键操作前必须验证用户身份，验证用户是否具备操作数据的权限直接对象引用的加密资源ID，防止攻击者枚举ID，敏感数据特殊化处理永远不要相信来自用户的输入，对于可控参数进行严格的检查与过滤
```

burpsuite中的intruder模式中各个选项的具体含义以及遍历方式

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMHGFbMIvJIzSkffnHZ2hxNcpOkaQpfOY4UI0Pibgqp0N8m06QCDgg6ibg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**1、sniper**会先对第一个变量进行字典遍历（第二个变量还是抓包之前输入的那个），然后对第二个变量进行字典遍历；例如之前的输入第二个为q。（只有一个payloadset值为1）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMWSXmrzm65iaOmddJ8GFN3r9tzB2OznkKdHbwq2ShIic05peZPdXt72pw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**2、batteringram**会将两个变量同时进行替换，替换同一个字典的值（只有一个payloadset值为1）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMLOw2zNyicPPaGCPFyVicOoWoMlYEoSbOVeSP3FFVDAKpjod2qecMyqlA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**3、pitchfork，**设置几个变量就会有几个payloadset值，会利用第一个字典里面的第一个值对第一个变量进行替换，会利用第二个字典里面的第一个值对第二个变量进行替换，利用第一个字典里面的第二个值对第一个变量进行替换，利用第二个字典里面的第二个值对第二个变量进行替换，利用第一个字典里面的第三个值对第一个变量进行替s换，利用第二个字典里面的第三个值对第二个变量进行替换，假如两个字典相同，就会产生下面的结果（一一相对）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMIVj9lMPAqa4sSianEUiapjx6IeGM2yYQvL2n64qmdVjcGoIP7HAzKNDQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**4、cluster bomb**会对两个字典进行所有组合（使用较多）option中的Grep-Match用来确定那种状态是成功的，哪一种是失败的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMxCryTBqjkN8EfUKj3U0Px1ic2etFZUibQ048w4icSqfG8VMB5zA1m8D5w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



flag出错误的内容

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMnnqo17uibDkPBVdLF54sgFLly4AqMITH5pewLSMjv7ibiaibOjickRsySsA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)