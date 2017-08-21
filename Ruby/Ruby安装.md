Ruby 对于 MAC 电脑的重要性不言而喻。Ruby 详情可了解其[中文官网](https://www.ruby-lang.org/zh_cn/)，这里主要介绍终端命令行中关于 Ruby 安装的相关操作。我们使用rvm 操作 ruby 的安装，想知道 rvm 的点这里[https://rvm.io](https://rvm.io)。

这里以 ruby 2.3.4 版本举例。

查看安装的 ruby 和版本号。

```rvm
rvm list
```

安装 ruby 2.3.4。

```rvm
rvm install 2.3.4
```

多个 runy 指定使用 2.3.4

```rvm
rvm use ruby-2.3.4
``` 

这里不建议安装多个 ruby, 建议卸载原来的版本 2.2.0

```rvm
rvm remove ruby-2.2.0
``` 

安装完毕之后防止出问题，建议更新一下 ruby 的插件库。

```gem
gem update
```

平时也可以通过此命令更新相关插件。当然安装新版本 ruby 后，pod 可能会碰见需要重新安装的情况。

&#160;

----------

#Appendix

##Related Documentation

[中文官网](https://www.ruby-lang.org/zh_cn/)

[Ruby教程](http://www.yiibai.com/ruby/)

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-06-29 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974