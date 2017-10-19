# 1 介绍

在执行程序时，Spring MVC 会根据客户端请求参数的不同，将请求消息中的信息以一定的方式转换并绑定到控制器类的方法参数中。这种将请求数据与后台方法参数建立连接的过程就是 Spring MVC 中的数据绑定。

在数据绑定中，Spring MVC 会通过数据绑定组件（DataBinder）将请求参数串的内容进行类型转换，然后将转换后的值赋值给控制器类中方法的形参。整个数据绑定的过程如下：

1. Spring MVC 将 ServletRequest 对象传递给 DataBinder。
2. 将处理方法的入参对象传递给 DataBinder。
3. DataBinder 调用 ConversionService 组件进行数据类型转换、数据格式化等工作，并将 ServletRequest 对象中的消息填充到参数对象中。
4. 调用 Validator 组件对已经绑定请求消息数据的参数对象进行数据合法性校验。
5. 校验完成后会生成数据绑定结果 BindingResult 对象，Spring MVC 会将 BindingResult 对象中的内容赋给处理方法的相应参数。

# 2 绑定简单数据

如请求网络 `http://localhost:8080/selectUser?id=1` 则会请求如下方法。

```java
@RequestMapping("/selectUser")
public String selectUser(Integer id);
``` 

这里参数 id 的值则自动注入到方法中。

# 3 绑定 POJO

创建一个 user 类。

```java
public class User {

    private Integer id;
    private String username;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }
}
```

浏览器输入  `http://localhost:8080/selectUser?id=1&username=阳君` ，则会自动调用如下方法。

```java
@RequestMapping("/selectUser")
public String selectUser(User user);
```

参数对象自动包装为 User 对象，并注入到相关属性 id 和 username 中。

# 4 绑定数组

在实际开发时，可能会遇到前端请求需要传递到后台一个或多个相同名称的情况，此种情况则可以绑定数组的方式解决。

```jsp
<form action="${pageContext.request.contextPath }/deleteUsers" method="post">
	<table width="20%" border=1>
		<tr>
			<td>选择</td>
			<td>用户名</td>
		</tr>
		<tr>
			<td><input name="ids" value="1" type="checkbox"></td>
			<td>tom</td>
		</tr>
		<tr>
			<td><input name="ids" value="2" type="checkbox"></td>
			<td>jack</td>
		</tr>
	</table>
	<input type="submit" value="删除"/>
</form>
```

当我们请求这样的表单时，Spring MVC 则会注入下面方法中的数组参数中。

```java
RequestMapping("/deleteUsers")
public String deleteUsers(Integer[] ids);
```

# 5 绑定集合对象

如前面的绑定数组只传入了一个参数，有的时候会遇到复杂的业务场景，我们还需要传入其他的信息。

```jsp
<form action="${pageContext.request.contextPath }/editUsers" method="post" id='formid'>
	<table width="30%" border=1>
		<tr>
			<td>选择</td>
			<td>用户名</td>
		</tr>
		<tr>
		  <td> <input name="users[0].id" value="1" type="checkbox" /> </td>
		  <td> <input name="users[0].username" value="tome" type="text" /> </td>
		</tr>
		<tr>
		  <td> <input name="users[1].id" value="2" type="checkbox" /> </td>
		  <td> <input name="users[1].username" value="jack" type="text" /> </td>
		</tr>
	</table>
	<input type="submit" value="修改" />
</form>
```

如批量修改指定id的用户名，这里我们需要一个 VO 对象做包装绑定。

```java
public class UserVO {
	private List<User> users;
	public List<User> getUsers() {
		return users;
	}
	public void setUsers(List<User> users) {
		this.users = users;
	}
}
```

UserVO 包装了 User对象，则 From 提交调用的方法如下所示。

```java
@RequestMapping("/editUsers")
public String editUsers(UserVO userList);
```

# 6 绑定自定义数据

多数情况，前面的绑定已经满足我们的需求，但是有的时候，我们会碰到特殊类型的转换，如日期数据的转换。数据的转换有三种方式，格式化（Formatter）、转换器（Converter）和注解（@DateTimeFormat）。三者的加载过程中，前面一旦转换成功，则不会调用后面的转换方法。

## 6.1 Formatter 

Formatter 的源类型必须是一个 String 类型，而 Converter 可以试任何类型。我们通过实现接口 Formatter 完成转换器的指定。

```java
public interface Formatter<T> extends Printer<T>, Parser<T> {

}
```

创建日期转换类 DateFormatter。

```java
public class DateFormatter implements Formatter<Date> {

    // 定义日期格式
    String datePattern = "yyyy-MM-dd HH:mm:ss";

    public String print(Date date, Locale locale) {
        return new SimpleDateFormat().format(date);
    }

    public Date parse(String source, Locale locale) throws ParseException {
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat(datePattern);
        return simpleDateFormat.parse(source);
    }
}
```

在 xml 中配置如下：

```java
<mvc:annotation-driven conversion-service="conversionService"/>
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="formatters">
        <set>
            <bean class="DateFormatter" />
        </set>
    </property>
</bean>
```

请求网络 `http://localhost:8080/customDate?date=2017-10-18 17:09:00` 则会请求如下方法，并完成自动转换。

```java
@RequestMapping("/customDate")
public String customDate(Model model, Date date);
```

## 6.2 Converter

Converter 转换类主要实现 org.springframework.core.convert.converter.Converter接口。接口如下，它可以接受任何参数类型并转换任何参数类型。

```java
@FunctionalInterface
public interface Converter<S, T> {
	@Nullable
	T convert(S source);
}
```

定义一个 DateConverter 日期转换器。

```java
public class DateConverter implements Converter<String, Date> {

    private String datePattern = "yyyy-MM-dd HH:mm:ss";
    
    public Date convert(String source) {
        SimpleDateFormat sdf = new SimpleDateFormat(datePattern);
        try {
            return sdf.parse(source);
        } catch (ParseException e) {
            throw new IllegalArgumentException(
                    "无效的日期格式，请使用这种格式:" + datePattern);
        }
    }
}
```

xml 的配置和 Formatter 类似。

```xml
<mvc:annotation-driven conversion-service="conversionService"/>
<bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="DateConverter"/>
        </set>
    </property>
</bean>
```

请求网络 `http://localhost:8080/customDate?date=2017-10-18 17:09:00` 则会请求如下方法，并完成自动转换。

```java
@RequestMapping("/customDate")
public String customDate(Model model, Date date);
```

## 6.3 注解

@DateTimeFormat 是一个很简单的日期转换注解，它大大的减少了我们的开发量，并提供了更多的格式支持。 只需在方法中如下所示，即可完成日期类型的转换。

```java
@RequestMapping("/customDate")
public String customDate(Model model, @DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") Date date)
```

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Related Documentation

[spring-framework-reference](https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/core.html)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-18 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974