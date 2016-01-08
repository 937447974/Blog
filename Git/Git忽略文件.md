在使用git开发过程中，我们会遇到这样一种场景，本地的文件不需要提交到版本仓库里面，如使用XCode过程中的xcuserdata文件夹。

#1 匹配规则

1. "/"表示目录。如忽略所有xcuserdata文件夹(xcuserdata/)、根目录下的xcuserdata文件夹(/xcuserdata/);
2. "\*"代表多个字符。如所有txt文档(*.txt);
3. "?"代表单个字符；
4. "[]"代表包含单个字符的匹配类别。如所有.m和.h文档(*.[mh]);
5. "!"代表取反。不忽略所有xcuserdata文件夹(!xcuserdata/)

#2 忽略xcuserdata文件夹

下面是忽略xcuserdata文件夹的命令行。

```swift
echo "xcuserdata/" >> .gitignore
git add .gitignore
git commit -m "Add .gitignore"
git push
```

&#160;

----------

#其他

##文档修改记录

| 时间 | 描述 |
| ---- | ---- |
| 2016-01-08 | 博文完成 |

##版权所有

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog