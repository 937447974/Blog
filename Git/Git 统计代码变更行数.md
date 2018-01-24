1、通过git diff命令行

```
git diff --shortstat <commitid-1> <commitid-2>
```

2、通过 git log 命令行

先安装gawk

```
brew install gawk
```

设置起止时间，运行如下命令

```
git log --pretty=tformat: --since=2017-12-01 --until=2018-01-01   --numstat | gawk '{ add += $1 ; subs += $2 ; loc += $1 + $2 } END { printf "added lines: %s removed lines : %s total lines: %s\n",add,subs,loc }'
```

3、diff两个分支

```
git diff --stat driver_5.0.26 driver_5.0.28
```

4、统计成员贡献

```
git log --format='%aN' | sort -u | while read name; do echo -en "$name\t"; git log --author="$name" --since ==2018-01-14 --until=2018-01-15 --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 + $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -; done
```

&#160;

----------

# Appendix

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974