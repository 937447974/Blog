首先安利官网：https://brew.sh/index_zh-cn

第一步：创建HomeBrew文件夹

首先确保/usr/local/Homebrew文件夹不存在，存在的话删除，然后执行：

```
sudo mkdir /usr/local/Homebrew
```

第二步：git克隆

```
sudo git clone https://mirrors.ustc.edu.cn/brew.git /usr/local/Homebrew /usr/local/Homebrew
``` 

第三步：创建一个快捷方式到/usr/local/bin目录

```
sudo ln -s /usr/local/Homebrew/bin/brew /usr/local/bin/brew
```

如果提示File exists表示/usr/local/bin文件夹里面已经有brew，删除后再运行第三步。

第四步：创建core文件夹 并 再次git克隆

```
sudo mkdir -p /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core
```

以及

```
sudo git clone https://mirrors.aliyun.com/homebrew/homebrew-core.git /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core
```

第五步：获取权限 并 运行更新

```
sudo chown -R $(whoami) /usr/local/Homebrew
以及
```

brew update
稍等一会儿～大功告成！

最后设置：设置环境变量，再运行下面两句后，重启终端：（命令中的链接地址可以替换为第二步或者第四步中对应的链接地址）

echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.zshrc 
 
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles' >> ~/.bash_profile
brew有一个自检程序，如果有问题自检试试：

brew doctor
查看全部安装路径

brew list
查看指定软件安装路径

brew list 软件名
另外如果已经用官网的命令成功安装好了Homebrew的童鞋（好吧果然你们有耐心。。。），可以通过替换镜像源来解决安装软件慢以及更新慢的问题：

当然如果是通过本文介绍的安装方法是不用替换的

替换brew.git:（命令中的链接地址可以替换为第二步中对应的链接地址）

cd "$(brew --repo)"
 
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
 
替换homebrew-core.git:（命令中的链接地址可以替换为第四步中对应的链接地址）

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
 
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
 
如果想要重置回默认的源：

重置brew.git:

cd "$(brew --repo)"
 
git remote set-url origin https://github.com/Homebrew/brew.git
重置homebrew-core.git:

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
 
git remote set-url origin https://github.com/Homebrew/homebrew-core.git


