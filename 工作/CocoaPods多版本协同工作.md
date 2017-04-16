公司的项目多数情况下是指定Cocoapods版本的，但是自己又想使用最新的Cocoapods库。难道要一会儿升级到最新的Cocoapods，一会儿降级到公司的Cocoapods版本！！！

接下来为大家介绍两种方案

## 1 直接执行

带版本执行相关命令

```
pod _1.0.0_ update //pod _version_ update
```

## 2 项目级Cocoapods

第一种方法暴力直接，但需记住工程对应的Cocoapods版本号，否则。。。

接下来为大家介绍一种比较优雅的方式，为每个工程指定Cocoapods版本。

大家知道Cocoapods的核心是一个叫做Podfile的文件，通过在Podfile上写入项目所需pod的配置，我们可以通过简单的pod install pod update命令来集成项目所需的pod。

Cocoapods的这个思路其实是借鉴了一个叫做Bundler的工具，而Bundler就是实现第二种方案的关键。具体步骤如下：

1. 安装Bundler：Bundler本身就是一个gem，通过gem install bundler命令即可安装
2. 类似Cocoapods的Podfile文件，我们需要创建一个Gemfile文件，文件位置和Podifle所在位置相同即可。（通过在项目主目录下执行bundle init命令也可）
3. 在Gemfile文件中，我们想配置所需pod一样配置我们所需的gem：

	```
	source "https://rubygems.org"
	gem 'cocoapods', '1.0.0'
	```

	和pod install一样的，执行bundle install


想要运行刚刚Bundler安装的cocoapods的话，在相应位置，执行bundle exec pod install即可（除了有bundle exec这个前缀，其他平时使用pod命令一样。去掉bundle exec这个前缀，运行的就是全局安装的Cocoapods）

&#160;

----------

# Appendix

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-12-08 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974