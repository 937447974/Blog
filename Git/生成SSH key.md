使用git的过程中，我们会初始化创建关联服务器的SSH key.

# 1 设置用户名和邮箱

开发过程中，提交的时候会在log中显示用户名和密码，便于管理。

```
$ git config --global user.name "阳君"
$ git config --global user.email "937447974@qq.com"
```

# 2 检查现有的SSH keys

在创建SSH keys之前，我们可以看看电脑内是否有SSH keys秘钥。

打开Terminal输入如下命令。

```
$ ls -al ~/.ssh
```

或输入

```
$ ls ~/.ssh
```

如果看见如下文件，则代表SSH keys已创建好。

1. id_dsa.pub
2. id_ecdsa.pub
3. id_ed25519.pub
4. id_rsa.pub

查看已创建好的SSH key，使用如下命令。

```
$ cd ~/.ssh
$ cat id_rsa.pub
```

# 3 生成新的SSH key

如果没创建SSH key，我们可以创建新的SSH key。

1 设置电子邮件并创建对应的key 

```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
# Creates a new ssh key, using the provided email as a label
Generating public/private rsa key pair.
```

2 设置文件存储位置，直接“回车”。

```
Enter a file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]
```

3 设置密码时，可设置空密码。

```
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

# 4 添加SSH key到ssh-agent

```
# start the ssh-agent in the background
eval "$(ssh-agent -s)"
Agent pid 59566

$ssh-add ~/.ssh/id_rsa
```

此时SSH key创建完毕。

&#160;

----------

# Appendix

## Related Documentation

[Generating an SSH key](https://help.github.com/articles/generating-an-ssh-key/)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-03-01 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974/Blog