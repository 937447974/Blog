Ruby 中大量使用了块(block)，block 使得我们的代码更加精简，更加具有维护性。

# 1 语法

块的调用方法一般采用以下形式：

```ruby
对象.方法名(参数列表)|块变量|	块代码
end
```

或者

```ruby对象.方法名(参数列表){|块变量|  
	块代码}
```

# 2 块回调

## 2.1 Yield

yield 主要用于隐式 block 回调，ruby 方法默认可以不声明 block，在其内部可通过 yield 回调。

```ruby
def test
    yield("YJ") if block_given? # block_given?判断是否存在 block
end

test{|str| puts str}
``` 

## 2.2 Call

如果方法的最后一个参数前带有 &，那么可以向该方法传递一个块，且这个块可被赋给最后一个参数。如果 * 和 & 同时出现在参数列表中，& 应放在后面。block 的参数使用 call 回调。

```ruby
def test(&block)
    block.call("YJ") if block
end

test{|str| puts str}
```

## 2.3 Proc

有的时候，我们希望传入一个方法多个 block，或将block转化为一个属性，可以使用 Proc 对象。

```ruby
def test(block1, block2)
    block1.call("Y") if block1
    block2.call("J") if block1
end

block1 = Proc.new do |str|
    puts str
end

block2 = Proc.new{|str|
    puts str
}

test(block1, block2) # 传入 block 临时变量
```

# 3 BEGIN 和 END 块

每个 Ruby 源文件可以声明当文件被加载时要运行的代码块（BEGIN 块），以及程序完成执行后要运行的代码块（END 块）。

```ruby
BEGIN {
    puts "BEGIN 代码块"
}

END {
    puts "END 代码块"
}
puts "MAIN 代码块"
```

&#160;

----------

# Appendix

## Related Documentation

[Ruby 块 | 菜鸟教程](http://www.runoob.com/ruby/ruby-block.html)

[聊聊 Ruby 中的 block, proc 和 lambda](https://ruby-china.org/topics/10414)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-07-31 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974