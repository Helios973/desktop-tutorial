# XD安全 学习笔记 | Java

原创 Rel1fe [0x00实验室](javascript:void(0);) *2021-09-04 06:55*

收录于合集#小迪安全笔记20个

声明

作者：团队成员-Rel1fe 【无名安全团队】 如需转载本实验室的文章，请标明来源即可。文章仅学习安全技术使用，切记请勿做它用，产生的后果与本公众号无关。



本文为day40-41天 Java安全的学习笔记。



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaTCvHAgFdUReykCAcdp3ucgjOJiblQ8l9iatWzDdv6UzTp6paaWMRyCpQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**JWT****定义** 

JWT（Json Web Token）是为了在网络应用环境间**传递声明**而执行的一种基于JSON的开放标准。JWT的声明一般被用来在身份提供者和服务提供者间传递被认证的用户身份信息，以便于从资源服务器获取资源，也可以增加一些额外的其它业务逻辑所必须的声明信息，该token也可直接被用于认证，也可被加密。



其工作流程如下图：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xablSlJvc6F7ZBzgFyGz1kvhLcydCTZ3lIFS3uxLFqo1MhiaTZa5UicqVA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



1、用户端登录，用户名和密码在请求中被发往服务器

2、（确认登录信息正确后）服务器生成 JSON 头部和声明，将登录信息写入 JSON 的声明中（通常不 应写入密码，因为 JWT 是不加密的），并用 secret 用指定算法进行加密，生成该用户的 JWT。此时， 服务器并没有保存登录状态信息。

3、服务器将 JWT（通过响应）返回给客户端

4、用户下次会话时，客户端会自动将 JWT 写在 HTTP 请求头部的 Authorization 字段中

5、服务器对 JWT 进行验证，若验证成功，则确认此用户的登录状态6、服务器返回响应



JWT与session**区别**：

两者的主要目的都是存储用户信息，但是session将用户信息存储再服务器端，而JWT则是在客户端。JWT方式将用户状态分散到了客户端中，可以明显减轻服务端的内存压力。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaIRxQLlXsaWbqWVbEyicQagfSAHicBILCia4nevlt3aCYibmuzqutMkHibYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaN9CKSPeSuDnWtJd8oRon7RQPa2U40jBdI9YX9KKNf8Pgmtrhen58Wg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**JWT****格式**

首先看JSON Web Tokens - jwt.io给出的示例：



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xauib8iayzfTyGm8AzwH5iaHOcibcnBzTjK0WJ7HUlQO2zicAiaUSkibjc0hHCg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



由图可知JWT由三部分组成：

- 

```
header.payload.signature
```

**HEADER**

头部包含两个部分ALGORITHM & TOKEN TYPE 

  alg 说明这个JWT的签名使用的算法的参数，常见值用HS256（默认），HS512等，也可以为None。HS256 表示 HMAC SHA256。 

  typ 说明这个 token 的类型，此例中为 JWT



**payload**

载荷就是存放有效信息的地方。这些有效信息包含三个部分

- 标准中注册的声明
- 公共的声明
- 私有的声明



**标准中注册的声明** (建议但不强制使用) ：

**iss**: jwt签发者

**sub**: jwt所面向的用户

**aud**: 接收jwt的一方

**exp**: jwt的过期时间，这个过期时间必须要大于签发时间

**nbf**: 定义在什么时间之前，该jwt都是不可用的.

**iat**: jwt的签发时间

**jti**: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。



**signature**

签证信息由三部分组成：

header (base64UrlEncode后)

payload (base64UrlEncode后) 

secret



**Base64URL****编码**

在HTTP传输过程中，Base64编码中的"=","+","/"等特殊符号通过URL解码通常容易产生歧义，因此产生了与URL兼容的Base64URL编码在Base64URL编码中，"+"会变成"-"，"/"会变成"_"，"="会被去掉，以此达到url safe的目的。



**Refresh token** 

JWT使用refresh token去刷新access token而无需再次身份验证。 refresh token的存活时间较长而access token的存活时间较短。



登陆时会获取 access token, refresh token：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xamsdrjPHmrAM4Mwqy9NUhtFgqQJj8HtLam1Z7wc18P3BLf7ibHbnUO1g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



服务器中**可能存在**：未校验access token和refresh token是否属于同一个用户，导致A用户可使用自己的refresh token去刷新B用户的access token



**实例****webgoat JWT** **lesson5****（未验证签名算法）**

此关漏洞出现的原因是后端解析了JWT却**没有验证**是否是使用自己的密钥签发的JWT。

此关需要获取admin权限，重置选票。游客没有投票权限，普通用户有投票权限没有重置投票权限。切换到Tom用户后点击重置投票后抓包，

分析JWT内容如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xabpdxIECFdMUMwEbTbBaicswDjY2casfqBPen1BoZVnQGIMM6nFbUQZg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xawZ6qibMOPoLW3KXfTEUTF4LGJtB2sSYrk2lvR0eH9zq4kee0HXWljFA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



猜测admin值为判断是否是管理员的依据，于是将admin值修改为true，alg值修改为"none"后进行base64Url编码，拼接成新的JWT发送数据包：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaMnkSSQTIWuKia6UAPIPDEU5aRG0s2W99FKGI4bv68pxZPpia7zZiaadLg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**lesson8****（爆破）** 

JWT使用HMAC（消息验证码）。密码的强度决定了JWT的安全性，使用关卡提示的字典爆破即可得到secret="victory"。



**lesson10****（****refresh token****越权刷新****access token****）**

思路是使用Jerry的refresh token和Tom过期的access token去获取Tom新的access token。复现有问题，

参考文章：



- 

```
https://www.freebuf.com/vuls/216457.html
```



**lesson11-****结合****sql****注入**

**Jerry想要删除Tom的账户，首先点击删除Jerry的账户抓包分析JWT如下：HEADER：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaq1bLpv25IBjUuSj2pibuwxewtN7Tg4fc68mxcYzcgRg6OR78J0mNFrw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



PAYLOAD：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaSIpoM2tp7ZldYiaTUh2nuBAQddAyaXwCwYRYsbksd3gpDsJbuQUl3ZQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



此关存在注入的原因是**直接读取了****kid****的值**进行了sql语句的**拼接**：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xa6AF8XcTp0L5LL6wZ09kiaKxIbSLUzeJrRia1UM69emJ2wZqvwR0eazfw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



通过阅读源码我们发现，在解析JWT时，使用的密钥是先根据header中kid的值在数据库中查询出的。最后返回密钥的base64编码。



构造sql语句如下：123' and 1=2 union select id FROM jwt_keys WHERE id='webgoat_key



通过脚本生成token：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaBOic8Vm89iaWKSJh81Z1HX6NicntmjOE9AibCJatZE9km31HxcDwnNTXMg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaHPxIWvTbmy8RAB6yic37ziaa9HufwYhLiaK8zjdiax5qrmyGpFiaF1bUqXw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



点击删除Tom按钮，抓包后修改Token即可：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaqFGo4XOWE30uFO6AlYTeAzUMdtLWA93M4bZg1wdB0vCHjkzcewg86Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**总结**

- 
- 
- 
- 

```
1.对JWT，signature key爆破和篡改JWT的写法需要根据源码来相应设置。2.对JWT，signature key爆破可尝试直接明文和base64encode两种（不排除其他种可能）；上文例子中，对明文key进行base64decode后作为signature key来签名，这种情况非常少见。3.refresh_token越权篡改他人access_token问题值得注意，refresh_token出现频率低，测试人员漏测几率高。4.可在JWT的headers，payload部分的参数值中插入常见漏洞相关payload去尝试，尽管我们不知道signature key。
```



**修复建议：**

修复算法，不允许客户端切换算法。

使用对称密钥对令牌进行签名时，确保使用适当的密钥长度。

确保添加到令牌的声明不包含个人信息。如果需要添加更多信息，也可以选择加密令牌。向项目添加足够的测试用例以验证无效令牌实际上不起作用。



------

**Java安全处理-\**sql\**\**预编译\****

**sql****注入产生的原因**：未经检查或者未经充分检查的**用户输入数据**，**意外变成了代码被执行**。SQL注入的**本质**就是利用SQL**拼接存在的缺陷**进行攻击。



**sql****预编译原理** 

在JDBC编程中，PreparedStatement 用于执行参数化查询。



PreparedStatement 对象所执行的 SQL 语句中，参数用问号?来表示，调用PreparedStatement 对象的 setXXX方法来设置这些参数。setXXX 方法有两个参数，第一个参数是要设置的 SQL 语句中的参数的索引(从 1 开始)，第二个是设置的 SQL 语句中的参数的值。



在对PreparedStatement进行预编译时，命令会带着占位符被数据库进行**编译**，并放到命令缓冲区。然后，每当执行同一个PreparedStatement语句的时候，由于在缓冲区中可以发现预编译的命令，虽然会被再解析一次，但不会被再次编译。

**
**

**编译过程**识别了关键字、执行逻辑之类的东西，编译结束了这条SQL语句能干什么就**确定了**。编译之后通过setXXX设置的部分，**无法再改变执行逻辑**，这部分就只能是相当于输入字符串被处理。



经典Java预处理代码如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaPMaPaKm0EUtHBgAHkkwWjYrRDfjZBlibx7bcCvqBeFIAanHpkTSu5vQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**预处理无法参数化的地方**

典型的就是**order by****后的参数**无法用占位符代替。

order by后一般是接字段名，而字段名是不能带引号的，比如 order by username；如果带上引号成了order by 'username'，那username就是一个字符串不是字段名了，这就产生了语法错误。



本质上来说：凡是字符串但又不能加引号的位置都不能参数化；包括sql关键字、库名、表名、字段名、函数名等等。order by后常常接字段名。

同理，**like**后也无法使用占位符代替



实例：webgoat SQL Injection (mitigation) order by 注入

  webgoat8.2.1版本中SQL Injection (mitigation)第12关，要求是找出服务器ip的前三位

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaLFFdqzlLK0lyeedqsCvdLricLB89puXeuO4IIRzib3qhbc1xwnbk1oxQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



题目已经给出了很明显的提示使用order by进行注入。通过第11课的学习我们知道了可以在order by 后使用如下语句进行注入：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaeIKPEq7S8oOTSsnEzktXnWSC8mIFic8mngApG1I1KicfMmaKXRnvtJ6Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



通过点击hostname、ip、mac等字段发现记录的排列顺序发生了变化，推测具有根据字段名进行排序的功能。

使用burpsuite抓包结果如下：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xamgicRvPVibguwUsIxMo9xskx70Xo4kJn8Plk1oTMWV0SHrrlibBmIVatw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在将参数 ip 修改为 ip' ，爆出了错误信息：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaFOxhCSy9hNGrXyCNy41JzYpaMwicqYvsyHnJQA4xWmxOkGA6g73zrfw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用上述的order by语句测试是否存在sql注入,返回结果会随着true、false而改变，说明存在注入点

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xazfXOHK9r0DfcDkZ0g8PP9Q3cBhlZKmGpT538OJQIhBmCYFZRAIibOBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**方法****1****：手工或者写脚本**

**由于只要求求前三位ip所以使用手工二分法所需的时间也不是很多，payload如下：**

- 

```
(case%20when%20(substring((select%20ip%20from%20servers%20where%20hostname%3d'webg oat-prd')%2c1%2c1)%3d1)%20then%20hostname%20else%20id%20end)
```

**也可通过python写脚本来实现信息获取。**

***\*
\****

***\*burpsuite\**\**爆破\****

**使用intruder模块，添加两个爆破点**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xauIdj0K5YHSALDA1VwvjWeo6aicUL4ibiaMofoDfuERT65b2KnkrBVB9Vw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**第一个设置从13、第二个设置从09。最后分析结果为hostname的界面为正确的结果104：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaoSXwicf7SmialELUrzxRImnQbBUeQzWjerD3T9ROdsfRoqIicWx2vJ71w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)