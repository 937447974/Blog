许多时候，在使用 Git 时，可能会因为某些原因想要修正提交历史。Git 允许重写已经发生的提交，这可能涉及改变提交的顺序，改变提交中的信息或修改文件，将提交压缩或是拆分，或完全地移除提交。

# 1 修改最后一次提交

修改最近一次提交可能是所有修改历史提交的操作中最常见的一个。对于最近一次提交，往往想做两件事情：修改提交信息，或者修改你添加、修改和移除的文件的快照。

```
$ git commit --amend
```

这会把你带入文本编辑器，里面包含了你最近一条提交信息，供你修改。当保存并关闭编辑器后，编辑器将会用你输入的内容替换最近一条提交信息。

使用 Sourcetree 打开项目会发现有两个 master，检查最新的 master 分支无误后，执行强制推送远端命令。

```
$ git push origin master -f
```

# 2 修改多个提交信息

修改在提交历史中较远的提交，必须使用更复杂的工具。 Git 没有一个改变历史工具，但是可以使用变基工具来变基一系列提交，基于它们原来的 HEAD 而不是将其移动到另一个新的上面。可以通过给 git rebase 增加 -i 选项来交互式地运行变基。 必须指定想要重写多久远的历史，这可以通过告诉命令将要变基到的提交来做到。

修改最近的两次提交

```
$ git rebase HEAD~2 -i
```

> 也可以直接指定 commit 跳转 `git rebase f20830d -i`

运行这个命令会在文本编辑器上给你一个提交的列表，看起来像下面这样：

```
pick cedeedc no1 message
pick f20830d no message

# Rebase 3d1e0ad..f20830d onto 3d1e0ad (2 commands)
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

需要注意的是这里展示的提交顺序是由 HEAD~2 到 HEAD。修改脚本可以将 pick 修改为 Commands 提示的如 reword、edit 修改提交的日志。还可以使用 squash、fixup 合并提交。

将倒数第二个提交，使用 edit 编辑后，如下所示。

> 如果只修改信息建议使用 reword 命令。

```
edit cedeedc no1 message
pick f20830d no message
```

当保存并退出(:wq)编辑后，Git会提交如下信息 

```
$ git rebase HEAD~2 -i
Stopped at 57f3a5b...  no1yj message
You can amend the commit now, with

  git commit --amend 

Once you are satisfied with your changes, run

  git rebase --continue
```

输入

```
$ git commit --amend
```

编辑提交信息，然后，运行。

```
$ git rebase --continue
```

这个命令将会自动地应用另外一个提交，然后就完成了。

最后使用 Sourcetree 打开项目，检查最新的 master 分支和对应的 tag 无误后，执行强制推送远端命令。

# 3 修改提交邮箱地址

通过 filter-branch 可以一次性修改多个提交中的邮箱地址。需要注意只修改自己的邮箱地址，所以你使用 --commit-filter：

```
$ git filter-branch --commit-filter '
        if [ "$GIT_AUTHOR_EMAIL" = "schacon@localhost" ];
        then
                GIT_AUTHOR_NAME="Scott Chacon";
                GIT_AUTHOR_EMAIL="schacon@example.com";
                git commit-tree "$@";
        else
                git commit-tree "$@";
        fi' HEAD
```

&#160;

----------

# Appendix

## Related Documentation

[Git 工具 - 重写历史](https://git-scm.com/book/zh/v2/Git-工具-重写历史)

## Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)