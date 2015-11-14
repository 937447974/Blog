[MAC系统安装MySql](https://github.com/937447974/Blog/blob/master/数据库/MAC系统安装MySql.md)

[数据库的准则(范式)](https://github.com/937447974/Blog/blob/master/数据库/数据库的准则(范式).md)

[SQL基础](https://github.com/937447974/Blog/blob/master/数据库/SQL基础.md)

[利用SELECT检索数据](https://github.com/937447974/Blog/blob/master/数据库/利用SELECT检索数据.md)

[SQL内置函数](https://github.com/937447974/Blog/blob/master/数据库/SQL内置函数.md)

-----

MAC下安装MySql

先安装Homebrew
在命令行运行ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

会自动安装。安装完毕后，就可以执行下列命令

1. brew install mysql 安装mysql
2. brew uninstall mysql 卸载mysql
3. brew update 更新所有软件
4. brew list查看安装了那些软件
5. brew upgrade mysql 更新mysql


这里我们使用brew install mysql安装mysql。

安装完毕后会提示你安装java se。下载地址：http://java.com/zh_CN/download/mac_download.jsp，MySql的运行是依赖java的。

在命令行mysql安装完毕后，你会看到如下输出：

```
A "/etc/my.cnf" from another install may interfere with a Homebrew-built
server starting up correctly.

To connect:
    mysql -uroot

To have launchd start mysql at login:
  ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
Then to load mysql now:
  launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
Or, if you don't want/need launchctl, you can just run:
  mysql.server start
```

此时你可以执行如下命令对mysql操作。

1. mysql -uroot 连接启动mysql；
2. ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents 将mysql设为开机启动；
3. launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist 如果你执行了上面的代码可以执行这段命令启动mysql服务；
4. mysql.server start 启动mysql服务；
5. mysql.server stop 关闭mysql服务；
6. ps -ef|grep mysql 查看mysql服务。