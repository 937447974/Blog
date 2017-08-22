File 表示一个文件的抽象类，通过它我们可以对任何文件进行读写操作。

# 1 Public Class Methods

## 1.1 absolute_path

```ruby
absolute_path(file_name [, dir_string] ) → abs_file_name
```

将相对路径转换为绝对路径。

## 1.2 atime

```ruby
atime(file_name) → time
```

获取文件最后被访问的时间。

## 1.3 basename

```ruby
basename(file_name [, suffix] ) → base_name
```

返回 path 最后的文件名。

## 1.4 birthtime

```ruby
birthtime(file_name) → time
```

返回文件的创建时间。

## 1.5 blockdev?

```ruby
blockdev?(file_name) → true or false
```

如果指定的文件是块设备，则返回true。

## 1.6 chardev?

```ruby
chardev?(file_name) → true or false
```

如果 file_name 是一个字符设备，则返回 true。

## 1.7 chmod

```ruby
chmod(mode_int, file_name, ... ) → integer
```

改变 file_name 的权限模式。

## 1.8 chown

```ruby
chown(owner_int, group_int, file_name,... ) → integer
```

改变 file_name 的所有者和所属组。

## 1.9 ctime

```ruby
ctime(file_name) → time
```

返回 file_name 的最后一个更改时间。

## 1.10 delete

```ruby
delete(file_name, ...) → integer
```

删除 file_name 文件。

## 1.11 directory?

```ruby
directory?(file_name) → true or false
```

判断 file_name 是否是文件夹。

## 1.12 dirname

```ruby
dirname(file_name) → dir_name
```

返回 file_name 的目录部分，不包括最后的文件名。

## 1.13 empty?

```ruby
empty?(file_name) → true or false
```

返回 file_name 是否是空的。

## 1.14 executable?

```ruby
executable?(file_name) → true or false
```

file_name 是否为可执行文件。

## 1.15 executable_real?

```ruby
executable_real?(file_name) → true or false
```

如果 file_name 通过真正的用户权限是可执行的，则返回 true。

## 1.16 exist?

```ruby
exist?(file_name) → true or false
```

文件存在返回 true

## 1.17 expand_path

```ruby
expand_path(file_name [, dir_string] ) → abs_file_name
```

返回 file_name 的绝对路径

## 1.18 extname

```ruby
extname(path) → string
```

获取 path 的扩展名

## 1.19 file?

```ruby
file?(file) → true or false
```

判断 file 是否为一个常规文件。

## 1.20 fnmatch

```ruby
fnmatch( pattern, path, [flags] ) → (true or false)
fnmatch?( pattern, path, [flags] ) → (true or false)
```

判断路径是否匹配。

## 1.21 ftype

```ruby
ftype(file_name) → string
```

获取文件类型：

1. file：普通文件
2. directory：目录
3. characterSpecial：字符特殊文件
4. blockSpecial：块特殊文件
5. fifo：命名管道（FIFO）
6. link：符号链接
7. socket：Socket
8. unknown：未知的文件类型

## 1.22 grpowned?

```ruby
grpowned?(file_name) → true or false
```

如果 file_name 由用户的所属组所有，则返回 true。

## 1.23 identical?

```ruby
identical?(file_1, file_2) → true or false
```

如果命名文件是相同的，则返回true。

## 1.24 join

```ruby
join(string, ...) → string
```

返回一个字符串，由指定的字符“/”连接。

## 1.25 lchmod

```ruby
lchmod(mode_int, file_name, ...) → integer
```

等价chmod，但不遵循符号链接。因此它将更改与链接相关的权限，而不是链接所引用的文件。

## 1.26 lchown

```ruby
lchown(owner_int, group_int, file_name,..) → integer
```

等价chown，但不遵循符号链接。因此它将更改与链接相关的权限，而不是链接所引用的文件。

## 1.27 link

```ruby
link(old_name, new_name) → 0
```

创建一个到文件 old_name 的硬链接。

## 1.28 lstat

```ruby
lstat(file_name) → stat
```

和stat，但它只返回自身符号链接上的信息，而不是所指向的文件。

## 1.29 mtime

```ruby
mtime(file_name) → time
```

返回 file_name 的最后一次修改时间。

## 1.30 new

```ruby
new(filename, mode="r" [, opt]) → file
new(filename [, mode [, perm]] [, opt]) → file
```

通过 filename 创建一个 File 对象

## 1.31 open

```ruby
open(filename, mode="r" [, opt]) → file
open(filename [, mode [, perm]] [, opt]) → file
open(filename, mode="r" [, opt]) {|file| block } → obj
open(filename [, mode [, perm]] [, opt]) {|file| block } → obj
```

通过 filename 返回一个 File 对象，或将对象传入块中。

## 1.32 owned?

```ruby
owned?(file_name) → true or false
```

如果 path 由有效的用户所有，则返回 true。

## 1.33 path

```ruby
path(path) → string
```

获取路径。

## 1.34 pipe?

```ruby
pipe?(file_name) → true or false
```

判断 file_name 是否为一个管道。

## 1.35 readable?

```ruby
readable?(file_name) → true or false
```

file_name 是否可读。


## 1.36 readable_real?

```ruby
readable_real?(file_name) → true or false click
```

file_name 通过真正的用户权限是否可读的。

## 1.37 readlink

```ruby
readlink(link_name) → file_name
```

返回 link_name（软链接）所指向的文件。

## 1.38 realdirpath

```ruby
realdirpath(pathname [, dir_string]) → real_pathname
```

获取真实的文件路径名。文件可以是软链接。

## 1.39 realpath

```ruby
realpath(pathname [, dir_string]) → real_pathname
```

获取真实的文件路径名。文件不是软链接。

## 1.40 rename

```ruby
rename(old_name, new_name) → 0
```

修改文件名。

## 1.41 setgid?

```ruby
setgid?(file_name) → true or false
```

如果 file_name 设置了 group 权限位，则返回true。

## 1.42 setuid?

```ruby
setuid?(file_name) → true or false
```

如果 file_name 设置了 user 权限位，则返回true。

## 1.43 size

```ruby
size(file_name) → integer
```

返回文件大小，单位字节。

## 1.44 size?

```ruby
size?(file_name) → Integer or nil
```

返回文件大小，单位字节。如果文件不存在，则返回 nil。

## 1.45 socket?

```ruby
socket?(file_name) → true or false
```

如果 file_name 是一个 socket，则返回 true。

## 1.46 split

```ruby
split(file_name) → array
```

返回一个数组，包含 file_name 的内容，file_name 被分成 File::dirname 和 File::basename.

## 1.47 stat

```ruby
stat(file_name) → stat
```

返回 file_name 上带有信息的 File::Stat 对象。

## 1.48 sticky?

```ruby
sticky?(file_name) → true or false
```

如果设置了 file_name 的 sticky 位，则返回 true。

## 1.49 symlink

```ruby
symlink(old_name, new_name) → 0
```

为 old_name 创建软链接 new_name。

## 1.50 symlink?

```ruby
symlink?(file_name) → true or false
```

判断 file_name 是否为软链接。

## 1.51 truncate

```ruby
truncate(file_name, integer) → 0
```

## 1.52 umask

```ruby
umask() → integer
umask(integer) → integer
```

如果未指定参数，则为该进程返回当前的 umask。如果指定了一个参数，则设置了 umask，并返回旧的 umask。

## 1.53 unlink

```ruby
unlink(file_name, ...) → integer
```

删除 file_name 文件。

## 1.54 utime

```ruby
utime(atime, mtime, file_name,...) → integer
```

改变 file_name 的访问和修改时间。

## 1.55 world_readable?

```ruby
world_readable?(file_name) → integer or nil
```

判断 file_name 是否可读，可读时返回权限位数。

## 1.56 world_writable?

```ruby
world_writable?(file_name) → integer or nil
```

判断 file_name 是否可写，可写时返回权限位数。

## 1.57 writable?

```ruby
writable?(file_name) → true or false
```

判断当前操作者对 file_name 是否可写。

## 1.58 writable_real?

```ruby
writable_real?(file_name) → true or false
```

判断当前操作者对 file_name 是否可写。

## 1.59 zero?

```ruby
zero?(file_name) → true or false
```

返回 file_name 是否是空的。size = 0 时返回 true。


# 2 Public Instance Methods

## 2.1 atime

```ruby
atime → time
```

获取最后一次访问时间。


## 2.2 birthtime

```ruby
birthtime → time
```

获取创建时间

## 2.3 chmod

```ruby
chmod(mode_int) → 0
```

改变权限模式

## 2.4 chown

```ruby
chown(owner_int, group_int ) → 0
```

改变 user 或 group 权限

## 2.5 ctime

```ruby
ctime → time
```

获取最后一次更改时间

## 2.6 flock

```ruby
flock(locking_constant) → 0 or false
```

文件加锁

1. LOCK_EX：独占锁，只有一个进程可以访问。
2. LOCK_NB：锁定时不阻塞。可以使用逻辑或其他锁选项结合使用。
3. LOCK_SH：共享锁，多个进程同时对一个文件持有共享锁。
4. LOCK_UN：解锁。

## 2.7 lstat

```ruby
lstat → stat
```

返回自身 stat 信息

## 2.8 mtime

```ruby
mtime → time
```

获取修改时间

## 2.9 path

```ruby
path → filename
```

获取路径名

## 2.10 size

```ruby
size → integer
```

## 2.11 to_path

```ruby
to_path → filename
```

获取路径名

## 2.12 truncate

```ruby
truncate(integer) → 0
```

截断数据到 integer 字节。

&#160;

----------

# Appendix

## Related Documentation

[Class:File(2.4.1)](https://ruby-doc.org/core-2.4.1/File.html)

[Ruby File 类和方法](http://www.runoob.com/ruby/ruby-file-methods.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-08-23 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974