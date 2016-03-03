#1 CocoaPods简介

[CocoaPods](https://cocoapods.org)主要用于在IOS开发过程中管理第三方库，如JSONKit，AFNetWorking等等。无须我们手动添加相关库及其依赖关系。

#2 CocoaPods安装

##2.1 使用淘宝做Ruby镜像

下载CocoaPods需要使用翻墙，故使用淘宝做Ruby镜像。

终端输入如下命令。

```
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
$ gem sources -l
```

完成后会在终端看见如下，代表Ruby镜像仅是taobao。

```
*** CURRENT SOURCES ***

https://ruby.taobao.org/
```

##2.2 下载CocoaPods

安装版本CocoaPods使用如下命令

```
$ sudo gem install cocoapods
```

当然你也可以安装指定版本的cocoapods版本，如0.37.2。

```
$ sudo gem install cocoapods -v 0.37.2
```

#3 CocoaPods卸载

卸载CocoaPods很简单，使用如下命令即可。

```
$ sudo gem uninstall cocoapods
```

&#160;

----------

#Appendix

##Related Documentation

[CocoaPods](https://cocoapods.org)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-03 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog