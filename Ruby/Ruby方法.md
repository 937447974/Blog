Ruby 通常用 def 关键字定义方法，在 def 之后是新方法的名称，紧接着是方法体。

# 1 基础

# 1.1 语法

语法如下：

```ruby
def method_name [( [arg [= default]]...[, * arg [, &expr ]])]
	expr..
end
```

简单方法：

```ruby
def method_name 
	expr..
end
```

带参数方法：

```
def method_name (var1, var2)
	expr..
end
```

参数带默认值：

```ruby
def method_name (var1=value1, var2=value2)
	expr..
end
```

可变数量的参数：

```ruby
def method_name (*var)
	for i in 0... var.length
		puts "#{var[i]}"
	end
end
```

## 1.2 返回值

ruby 中的 return 语句用于从 ruby 方法中返回一个或多个值。

语法：

```ruby
return [expr[`,' expr...]]
# 单返回值
return value1
# 多返回值
return value1, value2
```


Ruby 中的每个方法默认都会返回一个值。这个返回的值是最后一个语句的值。也就是说不写 return 也会拿到值，如下所示：

```ruby
def method_name
	"YJ"
end
puts method_name
```

不过为了程序更加具有阅读性，建议写 return。


#2 元编程

使用元编程特性，还能用另外的方式定义方法。

##2.1 用 def 关键字为类添加方法

```ruby
class Quote
	def display
		puts "Ruby's normal method definition process."
	end
end
```

在程序中使用 def 关键字定义新方法，Ruby 会遵循下面三个步骤。

1. 把每个方法体编译成独立的 YARV 指令片段（这种情况发生在 Ruby 解析和编译程序的时候）。
2. 使用当前的词法作用域来获取类或模块的指针（这种情况发生在 Ruby 执行程序的时候遇到了 def 关键字）。
3. 在该类的方法中保存新的方法名——实际上市保存对应方法名的整数 ID 值。

##2.1 使用 def self 添加类方法 

```ruby
class Quote
	def self.display
		puts "Defining class methods using an object prefix."
	end
end
```

如果调用 Quote.class，则会返回 Class。所有的类本质上都是 Class 类的实例。元类是内部的概念，通常对 Ruby 程序来说是不可见的。 

## 2.3 使用 class << self 来定义类方法

```ruby
class Quote
	class << self
		def display
			puts "Defining class methods using a new lexical scope."
		end
	end
end
```

## 2.4 为单个对象实例添加方法

```ruby
class Quote
end

some_quote = Quote.new
def some_quote.display 
	puts "Defining class methods using Singleton classes."
end
```

在内部，Ruby 使用了被称为单类的隐藏类来实现这种行为，这就好比是单个对象的元类。区别如下：

1. 单类是 Ruby 内部创建的特殊隐藏类，用于容纳特定对象独有的方法。
2. 当对象本身就是类的情况下，元类就是单类。


##2.5 使用 class << 增加新的单类方法

```ruby
class Quote
end

some_quote = Quote.new
class << some_quote
	def display
		puts "Defining methods using Singleton classes in a lexical scope."
	end
end
some_quote.display
```

&#160;

----------

#Appendix

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-07-27 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974