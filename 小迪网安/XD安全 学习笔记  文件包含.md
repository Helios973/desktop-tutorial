# XD安全 学习笔记 | 文件包含

原创 阿丘不皮也不卡 [0x00实验室](javascript:void(0);) *2021-09-03 08:08*

收录于合集#小迪安全笔记20个

声明

作者：团队成员-阿丘不皮也不卡 【无名安全团队】 如需转载本实验室文章，标明来源即可。文章仅学习安全技术使用，请勿做它用！



本文是小迪day31-32文件包含的学习笔记。

1、文件包含的作用：将文件以脚本的格式执行（根据当前网站脚本类型）

2、各种语言造成文件包含漏洞的简要写法



第八个 C 语言那个，是包含远程文件，其余的是包含本地文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJiciczFON8x4nsN22adV12JqMqgDL6243TFdRUN5GV2I0viagFIesbz44HES2x3W7qQQsGXoHiaiaqf7cQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



有文件包含的各个脚本的代码

文件包含在 php 中，涉及到的危险函数有四个，分别是include()、include_once()、require()、require_once()。



区别如下：

include：包含并运行指定的文件，包含文件发生错误时，程序警告，但会继续执行。include_once：和 include 类似，不同处在于 include_once 会检查这个文件是否已经被导入，如果已导入，下文便不会再导入，直面 once 理解就是只导入一次。

require：包含并运行指定的文件，包含文件发生错误时，程序直接终止执行。require_once：和 require 类似，不同处在于 require_once 只导入一次。



3、文件包含漏洞成因：

⚫ 可控变量

⚫ 文件包含函数



4、include.php 中有包含函数，1.txt 内容位 phpinfo，filename=1.txt 传参，执行代码得到图 3，直接访问 1.txt 得到图 2

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xarDC2yj2zphRcFmFKs3s8pcXmkfWGCgzwQKMbxXt3NadjrBia3s0p1Yg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaysnhtwfVTAGhLYBYibjNOO4bkBobm54p0ncaJxDETMeaJcJHL8gAPvw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xanc2n9xdJFdkUoic40AMfSklVZpBAIhtKCZgWkuRjWmiaur39oiau3QbEw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



文件包含原理演示

5、检测是否存在文件包含漏洞

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaibdia5GK24wC6AAiaT4iaLUe3rKf8Py5iaEAmU4oIdKh5picPfS2js2snR0g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

6、类型远程包含：在代码中设置，allow-url-include 为 on，则可以远程包含，在 phpinfo 可以查看

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaKNF29siaO3HhrpPsgk1ckHMZ4r7Sh2rDXxkziczJIP44QB2OXicKhBqXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaUokbxz0K4GbicZMzQYLd3R4tsqjtD1FiaeuRnEQoY2EDcRMA8N7G64AQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

7、如果想要包含的文件不在当前目录，可以使用../返回上级

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaP7ickn8AyDlJ4POA7TS3CSsgq1v6nialoMUlA5YJMEV2XWyZxJiaUTKBQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



8、有限制绕过方法（借鉴文件上传漏洞绕过方法）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaSmrPHxXJ8MbHs33O7Xbq8ASmHryASoNzgu7U98YAib2MXoHF4ddCicPQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaCjgPUB7fZFHYhibeicEcKHtHcZVqNZMauoIQUuntcaK8h3ib0ejUeLWkA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xag40F8ax8rXNG8e51n8I327WS7Rk1wZico7sFgB0pl70JiapUuux4mBIA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

远程包含有限制



伪协议

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaETZos1Z6FqFxdKc6icoiaES4NDzfybnvyJewE43YlukDpE06DyLxREmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



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
如果 PHP 的配置选项 allow_url_include、allow_url_fopen 状态为 ON 的话，则include/require 函数是可以加载远程文件的，这种漏洞被称为远程文件包含漏洞（RFI） file://+路径：将文件以脚本执行data://php://filter 可以在执行代码前将代码换个方式读取出来，只是读取，不需要开启，读取源代码并进行 base64 编码输出，不然会直接当做 php 代码执行就看不到源代码内容了php://input?test=php://input 【post data】<?php phpinfo();?>
```

用法：php://filter/read=convert.base64-encode/resource=要读取的文件

http://123.206.87.240:8005/post/index.php?file=php://filter/read=convert.base64-encode/resource=index.php（bugku 文件包含例题）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaLgF0s7dM8TKy6Tc0A6h5zdiaIL5LyDPnQwIx0erL6Zd6xiaMYF6jyjIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xa9TiboVVrjGUxKvm9yWghCeXI3OAFsFFmrtVzg6r1e1dWgNnWwUSEd6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现有 index.php?file=show.php，http://123.206.87.240:8005/post/show.php 发现返回内容一样,说明可能有文件包含，查看 show.php 没有内容，不管他，所以用php://filter 查看 index.php 文件，在注释中有 flag



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaibp6DBfpA98ic4cPV9ibWvy69eJm9UJDORlsxWIfzlw4P8SkBKzf5VxBw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



10、演示案例

确定漏洞为文件包含漏洞（检测）发现有 include，直接访问 phpinfo.php 发现页面一样，说明有文件包含（i 春秋）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaNfDXDn7U5o2icI2XdgCTpVnZk382EuGBXkibicPufGqRkCMqakfKc6OzQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xa5wEKhssTun9ZcRyV12spZq3S52GaTkqz6GX8ialVgcJ0UibRsW27djMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



先确定操作系统（Linux）,查看当前目录读取第一个文件，php://filter解码 base64 得到 flag

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaI119cGia2J6oicS6uaRWibH1ou6iaOicTEZyiaTmXjiaUXck6pBxPf85hULIQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**32 文件下载**

1、文件下载得作用：下载文件，凡是存在文件下载的地方都可能存在文件下载漏洞

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaCBSppJTx3icdiaPKGlpvDHMjictoicPlpLn4HS2aNcW5iaqib8LqTVyasZEw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2、下载数据库配置文件（敏感文件）

⚫ 扫描工具爬行或扫描地址

⚫ 下载好的文件代码中去分析路径（可见文件）和包含文件获取

3、直接访问和下载该文件是不一样的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaGiagTov1QG4j76FQfAh4h8GHcyv525RuStyD43ic7ZdAic7WS3Zjvg6XA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

4、演示案例

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaYyvsYHNX2fhSurciakiaGco6FUicJbZPZFAzpBNiaM5DWPDRVogrqUd5ag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



第二题：随便下载个东西

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaibERc5J9op11wmvVkbxJTzZa37foo7tlCW8gTLDPaBGACA6ibG6hhcVg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



同理：下载其他文件时，也要加密

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaU0RhmxLysOeiaGOZWIDVh98avHRkRcMkX6kgqCc4w5F3xic5jWQkaXibw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

文件下载漏洞在哪里测？

⚫ 有下载功能的地方文件下载漏洞怎么判断存在？

⚫ 下载 /index.php

⚫ 文件被解析，则是文件包含漏洞

⚫ 显示源代码，则是文件读取漏洞

⚫ 提示文件下载，则是文件下载漏洞，凡是有下载功能的地方都可能有下载漏洞



第四题：手工看参数值，发现点击 help 后有 filename=help.doc,报错，（因为脚本是 Java）可以改 post 请求，下载后发现什么也没有

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaichiavBhfoHPUS9W30ib67calr2xPWzoB8m8ztXYxW6ek0rhtZWWCsquQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



(JAVA WEB)先下载配置文件 ：WEB 配置文件 WEB-INF/web.xml，抓包

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaZqUkCicHvIS5yCYxYw0smficxH2GVibKaAO1YBVAmKon2iasPl7HyYx8lg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下载配置文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicnziceLhxypNgCgjEMpr0xaMH0fVGQSGonia2cXagd4HqicH08ibWptP69Qgg8zwmGXreomic0LH0FHVQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

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
根据上图修改路径下载文件：数据库、平台……配置文件WindowsC:\boot.ini //查看系统版本C:\Windows\System32\inetsrv\MetaBase.xml //IIS 配置文件C:\Windows\repair\sam //存储系统初次安装的密码C:\Program Files\mysql\my.ini //Mysql 配置C:\Program Files\mysql\data\mysql\user.MYD //Mysql rootC:\Windows\php.ini //php 配置信息C:\Windows\my.ini //Mysql 配置信息C:\Windows\win.ini //Windows 系统的一个基本系统配置文件
```



- 
- 
- 
- 
- 
- 
- 
- 

```
Linux/root/.ssh/authorized_keys/root/.ssh/id_rsa/root/.ssh/id_ras.keystore/root/.ssh/known_hosts //记录每个访问计算机用户的公钥/etc/passwd/etc/shadow/usr/local/app/php5/lib/php.ini //PHP 配置文件
```

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
/etc/my.cnf //mysql 配置文件/etc/httpd/conf/httpd.conf //apache 配置文件/root/.bash_history //用户历史命令记录文件/root/.mysql_history //mysql 历史命令记录文件/proc/mounts //记录系统挂载设备/porc/config.gz //内核配置文件/var/lib/mlocate/mlocate.db //全文件路径/porc/self/cmdline //当前进程的 cmdline 参数都可以尝试下载
```

收录于合集 #小迪安全笔记

 20个

上一篇XD安全渗透 学习笔记 | CSRF-SSRF阶段下一篇XD安全 学习笔记 | 逻辑漏洞