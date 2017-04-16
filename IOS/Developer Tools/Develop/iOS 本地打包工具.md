1、为什么要自动打包工具？

每修改一个问题，测试都让你打包一个上传fir ， 你要clean -> 编译打包 -> 上传fir -> 通知测试。而且打包速度好慢，太浪费时间了。如果有一个工具能自动的帮你做完上面所有的事情，岂不是快哉？

2、网上有那么多自动打包工具，我直接下载就行了为啥还要学习？

没错网上有很多打包工具，包括github上也有一些直接从github下载并打包上传的，但是他们的不一定适合你，首先下载下来要配置各种参数，不会配，还有网上大多是针对普通项目，但是我们项目是cocoaPods管理的，编译的是 xxx.xcworkspace 不是 xxx.xcodeproj 。怎么办 ， xxx.xcodeproj 自动编译后就在你项目目录下会有 xxx.app 但是 xxx.xcworkspace 找不到怎么办？怎么指定目录 ， 这些网上的库大都没有的。

3、需要哪些准备工作？

首先你得有装xcode , python3.5 （我装的版本,其他版本也行）, 待打包的项目。安装相关软件，随便搜索下就可以了。

废话结束，开始正文。本文介绍的是自动clean本地项目，编译 打包 上传fir 邮件通知相关人员。不涉及从git上下载。原理就是利用python执行控制台命令。对
iOS项目进行打包

# 1 xcode控制台命令

xcode 控制台命令基本都是以 xcodebuild 开头的介绍几个简单的命令，大家可以在命令行试试。

- `xcodebuild -version` 查看xcode的版本号和build的版本号
- `xcodebuild -showsdks` 显示当前系统的SDK、及其版本
- `xcodebuild -list` 先 cd 到工程目录下执行此命令 显示target Schemes 等

## 1.1 没有使用 cocoaPods 项目的编译

如果你的项目是普通的项目没有使用cocoaPods 那么 cd 到工程目录下直接执行 xcodebuild build ，就会自动编译了 参数都是默认 默认build release。

你也可以指定 `xcodebuild -configuration debug build build的时候会在你工程目录下生成一个build文件夹，build/Release-iphoneos/xx.app`

就是一会打包成ipa需要的文件。 第一次build速度会比较慢，要把编译环境拉下来，不要删除build文件夹，以后build 速度就会变快。

## 1.2 使用了 cocoaPods 项目的编译

如果不幸你也和我一样使用了cocoaPods , 其实也没啥不幸的 ，只是编译的时候就比较麻烦了 ，首先还是 cd 到项目目录 。但是你要指定编译文件和 scheme。而且还要指定build后build文件夹的位置，如果位置找不到，后面怎么自动打包ipa？。

我这里的命令大概是这样的：

`xcodebuild -workspace xxx.xcworkspace -scheme 你的scheme -configuration debug -derivedDataPath 指定路径 ONLY_ACTIVE_ARCH=NO`

这样就能正常编译并把build指定到我们想要去的目录

## 1.3 打包ipa

打包ipa只要上面路径对了，打包指定从.app 文件的路径 ， 打包到你指定地方就行了。

命令：

`xcrun -sdk iphoneos PackageApplication -v 这里填.app的路径 -o 指定存放ipa路径/文件名.ipa`

# 2 python实现篇

上面只是说了下编译的原理,下面看下怎么通过python自动处理这些任务 。

## 2.1 clean、编译、打包

首先创建一个xxx.py文件，需要你懂点python 语法，不懂就直接copy代码。不要改tab 。python的语法是严格按照tab区分的。后面我会放上我的代码，你们改改
变量就可以使用。

首先你需要引入一些外部依赖。设置编码为utf-8

```python
# -*- coding: utf-8 -*-
import os
import sys
import time
import hashlib
from email import encoders
from email.header import Header
from email.mime.text import MIMEText
from email.utils import parseaddr, formataddr
import smtplib
```

第一步 ， 声明一些变量

```python
# 项目根目录
project_path = "/Users/xx/project"
# 编译成功后.app所在目录
app_path = "/Users/xx/project/build/Build/Products/Release-iphoneos/xxx.app"
# 指定项目下编译目录
build_path = "build"
# 打包后ipa存储目录
targerIPA_parth = "/Users/xx/Desktop"
```

第二步，clean，和创建一个文件夹，这里的示例是针对有使用cocoaPods的项目 ， 如果没有使用 不用创建文件夹 ，命令自行简化

```python
# 清理项目 创建build目录
def clean_project_mkdir_build():
    os.system('cd %s;xcodebuild clean' % project_path) # clean 项目
    os.system('cd %s;mkdir build' % project_path) # 创建目录
```

%s 是py的占位符，字符串类型。后面是真正的填充。

第三步编译项目

```python
def build_project():
    print("build release start")
    os.system ('cd %s;xcodebuild -list')
    os.system ('cd %s;xcodebuild -workspace xxx.xcworkspace  -scheme xxx -configuration release -derivedDataPath %s ONLY_ACTIVE_ARCH=NO || exit' % (project_path,build_path))
```

不知道scheme是啥的xcodebuild -list 自己查

第四步 打包

```python
# 打包ipa 并且保存在桌面
def build_ipa():
    global ipa_filename
    ipa_filename = time.strftime('yourproject_%Y-%m-%d-%H-%M-%S.ipa',time.localtime(time.time()))
    os.system ('xcrun -sdk iphoneos PackageApplication -v %s -o %s/%s' % (app_path,targerIPA_parth,ipa_filename))
```
    
然后你现在再编写个方法，按顺序调用就可以编译打包了 ，执行完会看到桌面的ipa

```python
def main():
    # 清理并创建build目录
    clean_project_mkdir_build()
    # 编译coocaPods项目文件并 执行编译目录
    build_project()
    # 打包ipa 并制定到桌面
    build_ipa()
```

执行就在最下面直接调用就行了 main()

## 2.2 上传fir

我们是把代码上传到fir测试的，如果你们用的蒲公英或者其他，请自行搜索。

通过 `gem install fir-cli` 如果你没有ruby环境，自行搜索

安装完成后，在命令行输入fir 回车 。会有fir的命令提示。我们上传fir需要fir的API_TOKEN , 去fir官网登录找好就能找到。

拿到那一串串字符，在变量区加上

```python
# firm的api token
fir_api_token = "xxxxxxxxxxxxxxxxxxxxxxxxxx"
```

然后命令传入ipa目录和token就可以上传了

```python
# 上传
def upload_fir():
    if os.path.exists("%s/%s" % (targerIPA_parth,ipa_filename)):
        print('watting...')
        # 直接使用fir 有问题 这里使用了绝对地址 在终端通过 which fir 获得
        ret = os.system("/usr/local/bin/fir p '%s/%s' -T '%s'" % (targerIPA_parth,ipa_filename,fir_api_token))
    else:
        print("没有找到ipa文件")
```
    
这里也有遇到一个=坑，就是在终端直接fir 带后面的就可以执行 ，但是在这里识别不了命令，必须制定全路径，怎么找命令的全路径呢？终端输入`which fir`

## 2.3 发邮件

具体发邮件功能看代码，这里有几个变量。我使用的是新浪邮箱发的，smtp服务器 ， 如果你是 pop3 请更换，还要在邮箱里开启相应的服务

```python
from_addr = "xxxx@sina.com"
password = "*****"
smtp_server = "smtp.sina.com"
to_addr = 'aaa@qq.com,bbbb@qq.com'
```

然后发邮件的方法

我们的fir路径是固定的

```python
# 发邮件
def send_mail():
    msg = MIMEText('xxx iOS测试项目已经打包完毕，请前往 http://fir.im/xxxxx 下载测试！', 'plain', 'utf-8')
    msg['From'] = _format_addr('自动打包系统 <%s>' % from_addr)
    msg['To'] = _format_addr('xxx测试人员 <%s>' % to_addr)
    msg['Subject'] = Header('xxx iOS客户端打包程序', 'utf-8').encode()
    server = smtplib.SMTP(smtp_server, 25)
    server.set_debuglevel(1)
    server.login(from_addr, password)
    server.sendmail(from_addr, [to_addr], msg.as_string())
    server.quit()
```
    
然后执行顺序是这样的

```python
def main():
    # 清理并创建build目录
    clean_project_mkdir_build()
    # 编译coocaPods项目文件并 执行编译目录
    build_project()
    # 打包ipa 并制定到桌面
    build_ipa()
    # 上传fir
    upload_fir()
    # 发邮件
    send_mail()
# 执行
main()
```

本文重点在自动打包命令上，python代码感兴趣的可以去Python教程 学习。

&#160;

----------

# Appendix

## Sample Code

[https://github.com/smalldu/autoipa](https://github.com/smalldu/autoipa)

## Related Documentation

[iOS 本地打包工具](http://stonedu.site/2016/08/17/iOS-本地打包工具/)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-08-19 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974