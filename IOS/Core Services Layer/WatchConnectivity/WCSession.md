

WCSession主要用于促进iOS和watchkit中的app交流信息，可快速立刻传递信息。当一边处于后台状态时，另一端传输数据，此时所有业务会在后台执行。

##Configuring and Activating the Session

创建和启动session可使用如下代码

```swift
if WCSession.isSupported() {
    let session = WCSession.defaultSession()
    session.delegate = self
    session.activateSession()
}
```

然后通过delegate获取相关信息。

##Supporting Communication with Multiple Apple Watches

当用户切换watch时会发生如下生命周期变化。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2016100901.png)




&#160;

----------

#Appendix

##Sample Code

[Swift](https://github.com/937447974/Swift)

##Related Documentation


##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-20 | 博文完成 |

##Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)