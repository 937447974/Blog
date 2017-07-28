Ruby 中有五种类型的变量：

- 局部变量：局部变量名以小写字母或下划线(_)开头；
- 类变量：类变量名以@@符号开头；
- 实例变量：实例变量名以@符号开头；
- 全局变量：全局变量名以$号开头；
- 常数：大写字母开头。

Ruby属性和其他语言一样也有自动生成getter或setter方法。如下所示

| 属性 | 读写 | 只读 |  
| --- | --- | --- |
| @@类属性 | cattr_accessor | cattr_reader |  
| @实例属性 | attr_accessor | attr_reader |

>cattr_accessor 只在 rails 框架中使用。

下面用 Quote 的 name 属性举例。

```ruby
class Quote
	class << self
		attr_accessor:name # 类属性
	end
	attr_accessor:name # 实例属性
	def display
		puts self.name # self get实例属性
		puts self.class.name # self get类属性
	end
end  

# setter
Quote.name = "Y"
quote = Quote.new
quote.name = "J"
quote.display
```

这里使用了 class << self 和 attr_accessor 实现了类属性的读写。

&#160;

----------

#Appendix

##Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2016-07-28 | 博文完成 |

##Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974