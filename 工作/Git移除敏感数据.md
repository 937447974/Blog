>本篇博文属于git的高级操作，欢迎大家阅读。

有时我们会不小心将敏感数据，如密码文件放到git仓库中。虽然可以重新提交git rm删除文件，但是文件仍然在git的回滚历史中存在(不知不觉感觉很危险)。还有的时候我们项目开发周期长了，git项目的版本库中会有很多垃圾数据，这些数据我们很明显的知道不需要了。

我们不必将git仓库删了，重新建立。那样代价太大，我们的版本库的历史记录也丢失了。为了解决这些问题，在本篇博文将讲解git的高级操作，移除敏感数据。

#1 场景演示

##1.1 创建项目

在GitHub创建一个测试项目Test

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111801.png)

并写好相关描述信息“移除敏感数据”，同时点击“Initalize this repository with a README”按钮。这样Git就会帮你初始化项目。完成后点击"Create repository"按钮。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111802.png)

点击“Create repository”按钮后就会来到这样的界面。这个是GitHub改版后的界面，感觉非常爽。在“DownLoad ZIP”处有三个按钮，分别是项目地址、使用桌面版GitHub控制项目、下载项目。

使用命令行的方式下载项目。

```git
cd /Users/yangjun/Desktop/
git clone https://github.com/937447974/Test.git
```

这里我们是把项目下载到桌面。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111803.png)

你还可以点击第二个桌面按钮，使用桌面版的GitHub将Test项目下载到桌面。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111804.png)

>如果你不知道怎么下载桌面版的GitHub，请阅读我的博文《[GitHub桌面版](https://github.com/937447974/Blog/blob/master/工作/GitHub桌面版.md)》。

##1.2 提交数据

在提交前使用代码`du -hs`查看文件夹大小，发现空项目只有112K。

现在我们在Test项目中创建一个文件夹，文件夹内包含几张照片。为了看出效果，我使用的高清图片。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2015111805.png)

使用命令行提交项目。

```git
yangjundeMac-mini:Test yangjun$ git add .
yangjundeMac-mini:Test yangjun$ git commit -m '提交'
yangjundeMac-mini:Test yangjun$ git push
```

##1.3 移除数据

提交git仓库后，我们将刚刚创建的test文件夹移除。

然后使用命令行提交移除数据。

```git
yangjundeMac-mini:Test yangjun$ git add .
yangjundeMac-mini:Test yangjun$ git commit -m '移除'
yangjundeMac-mini:Test yangjun$ git push
```

然后执行命令`du -hs`查看文件夹大小，发现项目还有21M。

此时问题来了，不管你怎么处理，版本库里面都有这21M数据。然后你clone项目的时候，这21M数据也跟着下载下来咯。

#2 移除敏感数据

##2.1 移除数据

移除版本库中的数据使用filter-branch命名。打开命令行敲入如下命令

```git
yangjundeMac-mini:Test yangjun$ git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch test/IMG_4933.jpg' --prune-empty --tag-name-filter cat -- --all
```

只需将“test/IMG_4933.jpg”改为你想删除的文件路径即可，文件路径是以项目为根目录。

如果你觉得这样一个一个删除太麻烦了，git也支持删除文件夹的方式，只需添加'-r',然后将“test/IMG_4933.jpg”改为“test”即可。

```git
yangjundeMac-mini:Test yangjun$ git filter-branch --force --index-filter 'git rm -r --cached --ignore-unmatch test' --prune-empty --tag-name-filter cat -- --all
```

> test改为.会清楚所有缓存和本地数据，无法版本回滚，谨慎使用。

##2.2 防止再次提交

有的时候项目中需要一定的数据，我们又不想提交到版本库里面，可以创建一个.gitignore文件，告诉git，这些数据不用提交。

```git
yangjundeMac-mini:Test yangjun$ echo "Rakefile" >> .gitignore
yangjundeMac-mini:Test yangjun$ git add .gitignore
yangjundeMac-mini:Test yangjun$ git commit -m "Add .gitignore"
```

##2.3 仔细检查

仔细检查你的项目代码，和分支。因为接下来的操作将不可逆转。

##2.4 提交git库

当你检查项目后，发现没有任何问题，则将命令提交到版本库中。

```git
yangjundeMac-mini:Test yangjun$ git push origin --force --all
```

##2.5 提交git tags

你还需要提交你的git tags，用以更新服务器的git tags。

```git
yangjundeMac-mini:Test yangjun$ git push origin --force --tags
```

##2.6 删除本地缓存

当你的小伙伴git pull后，发现项目没有问题，你再执行git的gc操作清理本地缓存。

```git
yangjundeMac-mini:Test yangjun$ git for-each-ref --format='delete %(refname)' refs/original | git update-ref --stdin
yangjundeMac-mini:Test yangjun$ git reflog expire --expire=now --all
yangjundeMac-mini:Test yangjun$ git gc --prune=now
```

完成上述操作后，再次使用命令`du -hs`查看文件夹大小，发现项目只有108k。此时即完成移除敏感数据的相关操作。

#3 BFG Repo-Cleaner

git也推荐使用[BFG Repo-Cleaner](https://rtyley.github.io/bfg-repo-cleaner/)操作git,有兴趣的小伙伴可以去学习一下。

```git
$ bfg --delete-files Rakefile
$ bfg --replace-text passwords.txt
```

#4 小结

本篇博文讲解了怎么在版本库中删除不需要的文件，给项目瘦身。如果你正在使用git，或你周围的小伙伴也在使用git，希望你向他们推荐这篇文章。下面是我清理项目前后的效果图。


&#160;

----------

#其他

##参考资料

[Remove sensitive data](https://help.github.com/articles/remove-sensitive-data/)

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2015-11-18 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog