Dir 是一个表示底层文件系统中目录的目录流，它提供了各种方法来列出文件系统的目录及其内容。

在文件系统中有两个特殊的虚目录，`.`表示当前目录，`..`表示父目录。

# 1 Public Class Methods

## 1.1 []

```ruby
Dir[ string [, string ...] [, base: path] ] → array
```

和 Dir.glob 类似，返回匹配成功目录数组。

## 1.2 chdir

```ruby
chdir( [ string] ) → 0
chdir( [ string] ) {| path | block } → anObject
```

将进程的当前工作目录更改为给定的字符串。如我们在终端执行 `cd` 命令。

## 1.3 children

```ruby
children( dirname ) → array
children( dirname, encoding: enc ) → array
```

返回匹配成功的除了 “.” 和 ".." 的目录数组。

## 1.4 chroot

```ruby
chroot( string ) → 0
```

改变根目录（只允许超级用户）。并不是在所有的平台上都可用

## 1.5 delete

```ruby
delete( string ) → 0
```

删除空的文件夹，如果文件夹内有文件，则会有 SystemCallError 崩溃。

## 1.6 each_child

```ruby
each_child( dirname ) {| filename | block } → nil click to toggle source
each_child( dirname, encoding: enc ) {| filename | block } → nil
each_child( dirname ) → an_enumerator
each_child( dirname, encoding: enc ) → an_enumerator
```

将输入文件夹下的每个目录（除了“.”和“..”），通过块或枚举输出。

## 1.7 empty?

```ruby
empty?(path_name) → true or false
```

如果指定的目录内是空的则返回 true，否则返回 false。

## 1.8 entries

```ruby
entries( dirname ) → array
entries( dirname, encoding: enc ) → array
```

返回一个数组，包含目录 path 中的文件名。

## 1.9 exist?

```ruby
exist?(file_name) → true or false
```

如果指定的文件是目录时，则返回 true。

## 1.10 foreach

```ruby
foreach( dirname ) {| filename | block } → nil
foreach( dirname, encoding: enc ) {| filename | block } → nil
foreach( dirname ) → an_enumerator
foreach( dirname, encoding: enc ) → an_enumerator
```

将输入文件夹下的每个目录（包含“.”和“..”），通过块或枚举输出。

## 1.11 getwd

```ruby
getwd → string
```

获取当前工作目录。

## 1.12 glob

```ruby
glob( pattern, [flags], [base: path] ) → array
glob( pattern, [flags], [base: path] ) { |filename| block } → nil
```

将匹配成功的目录，通过数组或块输出。

1. *：任何文件
	- *：匹配任何文件
	- c*：匹配c开头的文件
	- *c：匹配c结尾的文件
	- \*c*：匹配文件中包含c的文件
2. **：匹配任何文件夹
3. ?： 匹配任意单个字符
4. [set]：匹配集合中任意一个字符
5. {p,q}：匹配文字p或文字q
6. \：转义字符

## 1.13 home

```ruby
home() → "/home/me" click to toggle source
home("root") → "/root"
```

获取当前工作环境的根目录。

## 1.14 mkdir

```ruby
mkdir( string [, integer] ) → 0
```

创建指定目录，可指定目录权限。如果创建失败则返回 SystemCallError 错误。

## 1.15 new

```ruby
new( string ) → aDir
new( string, encoding: enc ) → aDir
```

通过文件地址生成 Dir 对象。

## 1.16 open

```ruby
open( string ) → aDir
open( string, encoding: enc ) → aDir
open( string ) {| aDir | block } → anObject
open( string, encoding: enc ) {| aDir | block } → anObject
```

根据path地址生成目录对象，也可以通过块输出。


## 1.17 pwd

```ruby
pwd → string
```

获取当前工作环境地址。

## 1.18 rmdir

```ruby
rmdir( string ) → 0
```

删除空的文件夹，如果文件夹内有文件，则会有 SystemCallError 崩溃。

## 1.19 unlink

```ruby
unlink( string ) → 0
```

删除空的文件夹，如果文件夹内有文件，则会有 SystemCallError 崩溃。


# 2 Public Instance Methods

## 2.1 close

```ruby
close → nil
```

关闭目录流。Ruby 2.3之后不建议使用。

## 2.2 each

```ruby
each { |filename| block } → dir
each → an_enumerator
```

为dir中的每个目录执行一次块，或返回目录枚举。

## 2.3 fileno

```ruby
fileno → integer
```

获取文件描述符。

## 2.4 inspect

```ruby
inspect → string
```

获取描述 dir 对象的字符串。

## 2.5 path

```ruby
path → string or nil
```

获取dir对象对应的文件路径

## 2.6 pos

```ruby
pos → integer
```

获取dir中的当前位置。

## 2.7 pos=

```ruby
pos = integer → integer
```

移动到d中的某个位置。

## 2.8 read

```ruby
read → string or nil
```

返回d的下一个条目

## 2.9 rewind

```ruby
rewind → dir
```

移动到第一个条目

## 2.10 seek

```ruby
seek( integer ) → dir
```

移动到 d 中的某个位置。

## 2.11 tell

```ruby
tell → integer
```

获取当前dir的位置

## 2.12 to_path

```ruby
to_path → string or nil
```

获取路径地址。


&#160;

----------

# Appendix

## Related Documentation

[Class:Dir(Ruby 2.4.1)](https://ruby-doc.org/core-2.4.1/Dir.html)

[Ruby Dir 类和方法](http://www.runoob.com/ruby/ruby-dir-methods.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-08-22 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974