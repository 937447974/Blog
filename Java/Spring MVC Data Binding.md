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