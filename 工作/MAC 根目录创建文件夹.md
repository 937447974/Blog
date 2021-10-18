方案： 在根目录下面，出现一个软链接目录data，然后真实目录地址是：/Users/yangjun/data/。这里先建立好真实目录

### 1 创建文件夹

在一个合适的位置，比如：/Users/yangjun/data

### 2 编辑conf文件

编辑一下/etc/synthetic.conf文件

```vim
sudo vi /etc/synthetic.conf
```

然后在里面写入自己希望的软连接对应关系，比如：

```
data    /Users/yangjun/data
```

> 中间是tab，而不是space，这一点非常重要，否则会识别失败的。

### 3 重启电脑

最后重启电脑，即可看见data文件夹