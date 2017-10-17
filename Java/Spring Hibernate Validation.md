数据校验是 Web 应用为了安全必须处理的步骤，Spring MVC 提供了两种方法来对用户的输入数据进行校验，一种是 Spring 自带的 Validation 校验框架，另一种是利用 JRS-303 验证框架进行验证。

在实际开发中我们不是使用 Spring 自带的框架，而是使用 JRS 相关验证框架（Hibernate validator）完成开发。 [Hibernate-validator](http://hibernate.org/validator/) 是根据 [Bean Validation](http://beanvalidation.org) 的参考实现，遵循 Bean Validation 2.0 规范，基于 JSR 380 实现。

# 1 Bean Validation website

Bean Validation 中内置的 constraint 如下：

| 注解 | 作用 |
| ---- | ---- |
| @Valid |被注释的元素是一个对象，需要检查此对象的所有字段值 |
| @Null | 被注释的元素必须为 null |
| @NotNull | 被注释的任何元素必须不为 null |
| @AssertTrue	 | 被注释的元素必须为 true |
| @AssertFalse | 被注释的元素必须为 false |
| @Min(value) | 	被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @Max(value)	 | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @DecimalMin(value) | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @DecimalMax(value) | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @Negative | 被注释的元素必须是一个负数 |
| @NegativeOrZero | 被注释的元素必须是负数或 0 |
| @Positive | 被注释的元素必须是一个正数 |
| @PositiveOrZero | 被注释的元素必须是一个正数或 0 |
| @Size(max, min) | 被注释的元素的大小必须在指定的范围内 |
| @Digits (integer, fraction) | 被注释的元素必须是一个数字，其值必须在可接受的范围内 |
| @Past | 被注释的元素必须是一个过去的日期 |
| @PastOrPresent | 被注释的元素必须是一个过去或当前的日期 |
| @Future | 被注释的元素必须是一个将来的日期 |
| @FutureOrPresent | 被注释的元素必须是一个将来或当前的日期 |
| @Pattern(value) | 被注释的元素必须符合指定的正则表达式 |
| @NotEmpty | 集合对象的元素不为0，即集合不为空，也可以用于字符串不为 null |
| @NotBlank | 只能用于字符串不为null，并且字符串trim()以后length要大于0 |
| @Email | 被注释的元素必须是一个有效的邮箱地址 |

# 2 Hibernate Validator

Hibernate Validator 完全遵循了 Bean Validation 的规范，并在其基础上有附加的扩展。

| 注解 | 作用 |
| ---- | ---- |
| @CreditCardNumber(ignoreNonDigitCharacters=) | 被注释的字符串必须通过 Luhn 校验算法，银行卡，信用卡等号码一般都用 Luhn 计算合法性 |
| @Currency(value=) | 被注释的 javax.money.MonetaryAmount 货币元素是否合规 |
| @DurationMax(days=, hours=, minutes=, seconds=, millis=, nanos=, inclusive=) |  被注释的元素不能大于指定日期 |
| @DurationMin(days=, hours=, minutes=, seconds=, millis=, nanos=, inclusive=) | 被注释的元素不能低于指定日期 | 
| @EAN | 被注释的元素是否是一个有效的 EAN 条形码 |
| @Length(min=, max=) | 被注释的字符串的大小必须在指定的范围内
| @LuhnCheck(startIndex= , endIndex=, checkDigitIndex=, ignoreNonDigitCharacters=) | Luhn 算法校验字符串中指定的部分 |
| @Mod10Check(multiplier=, weight=, startIndex=, endIndex=, checkDigitIndex=, ignoreNonDigitCharacters=) | Mod10 算法校验 |
| @Mod11Check(threshold=, startIndex=, endIndex=, checkDigitIndex=, ignoreNonDigitCharacters=, treatCheck10As=, treatCheck11As=) | Mod11 算法校验 |
| @Range(min=, max=) | 被注释的元素必须在合适的范围内 |
| @SafeHtml(whitelistType= , additionalTags=, additionalTagsWithAttributes=, baseURI=) | classpath中要有jsoup包 |
| @ScriptAssert(lang=, script=, alias=, reportOn=) | 检查脚本是否可运行 |
| @URL(protocol=, host=, port=, regexp=, flags=) | 被注释的字符串必须是一个有效的url |

# 3 Test

## 3.1 Manev 导包

使用 manev 导入相关包。 这里只展示 spring-mvc 和 hibernate-validator 包。

```xml
<dependencies>
    <!-- spring-mvc-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.0.0.RELEASE</version>
    </dependency>
    <!-- hibernate-validator -->
    <dependency>
        <groupId>org.hibernate.validator</groupId>
        <artifactId>hibernate-validator</artifactId>
        <version>6.0.2.Final</version>
    </dependency>
</dependencies>
```

## 3.2 国际化配置

Hibernate Validator 默认国际化文件是 ValidationMessages.properties，存放的位置在 org.hibernate.validator，我们可以在项目根目录增加此默认文件自动替换达 Hibernate 的资源文件达到国际化的目的。多数情况项目是插件开发，各大业务线是自定义国际化文件的。这里我们添加一个文件 ValidationMessages_zh_CN.properties。

```xml
username.NotEmpty = 用户名不能为空
password.NotEmpty = 密码不能为空
password.Size = 密码长度应在{min}-{max}个字符
```

## 3.3 Xml 配置

### 3.3.1 web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         id="yjcocoa"
         version="3.1">

    <!-- 配置spring mvc 前端核心控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-config.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!-- 系统默认页面-->
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

</web-app>
```

###3.3.2 springmvc-config.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns="http://www.springframework.org/schema/beans"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 配置视图解释器ViewResolver -->
    <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!-- 配置扫描器 -->
    <context:component-scan base-package="com.springmvc"/>
    <!--配置注解驱动  -->
    <mvc:annotation-driven/>

    <!--配置静态资源的访问映射，此配置中的文件，将不被前端控制器拦截 -->
    <mvc:resources location="/js/" mapping="/js/**"/>

    <!-- 配置校验器 -->
    <!--国际化校验资源-->
    <bean id="hibernateValidationMessages" class="org.springframework.context.support.ResourceBundleMessageSource">
        <property name="defaultEncoding" value="UTF-8"/>
        <property name="basenames">
            <list>
                <!--多模块自定义校验文档-->
                <value>ValidationMessages</value>
            </list>
        </property>
    </bean>
    <!--属性校验-->
    <bean id="validator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
        <!-- 校验器，使用hibernate校验器 -->
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
        <property name="validationMessageSource" ref="hibernateValidationMessages"/>
    </bean>
    <!--方法校验-->
    <bean class="org.springframework.validation.beanvalidation.MethodValidationPostProcessor">
        <property name="validator" ref="validator"/>
    </bean>

</beans>
```

除了对实例类的属性校验，我们还可以通过 MethodValidationPostProcess 对方法参数进行校验。这样我们就不需要分组校验，只需要自定义方法即可达到分组校验的效果，进一步精简了代码。Spring 衔接 Hibernate Validator 只需配置一个 validator Bean 元素即可，并制定 bean 名称 validator，spring mvc 会默认加载 validator 校验器。国际化主要通过 ResourceBundleMessageSource 加载资源文件并注入 LocalValidatorFactoryBean 中。

## 3.4 JSP 页面

页面主要是index.jsp，其中 success.jsp就不再展示。

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" %>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>登录</title>
</head>
<body>
<form action="${pageContext.request.contextPath }/validator/login"
      method="post">
    用户名：<input type="text" name="username" value=${user.username}> <span>${usernameValid}</span><br/>
    密&nbsp;&nbsp;&nbsp;码：<input type="text" name="password" value=${user.password}><span>${passwordValid}</span><br/>
    <input type="submit" value="登录"/>
</form>
</body>
</html>
```

页面使用 from 提交。并使用 `${}` 获取数据。

## 3.5 Java 类

### 3.5.1 PO类

```java
package com.springmvc.po;

import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Size;

public class User {

    @NotEmpty(message = "{username.NotEmpty}")
    private String username;

    @NotEmpty(message = "{password.NotEmpty}")
    @Size(min = 6, max = 9, message = "{password.Size}")
    private String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

}
```

这里可直接看到注解校验的好处，开发简单，可维护性很高。国际化资源文件的写法使用了 `{key}` 的方式。

### 3.5.2 控制器

```java
package com.springmvc.controller;

import com.springmvc.po.User;
import com.springmvc.service.ValidatorService;
import org.hibernate.validator.internal.engine.path.PathImpl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.validation.FieldError;
import org.springframework.validation.ObjectError;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.validation.ConstraintViolation;
import javax.validation.ConstraintViolationException;
import javax.validation.Valid;

/**
 * ValidatorController.java
 * <p>
 * Created by 阳君 on 2017/10/16.
 * Copyright © 2017年 springmvc. All rights reserved.
 */
@Controller
@RequestMapping("/validator")
public class ValidatorController {

    @Autowired
    private ValidatorService validatorService;

    @RequestMapping("")
    public String index() {
        return "validator/index";
    }

    @RequestMapping("/login")
    public String login(Model model, @Valid User user, BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            for (ObjectError objectError : bindingResult.getAllErrors()) {
                FieldError fieldError = (FieldError) objectError;
                model.addAttribute(fieldError.getField() + "Valid", objectError.getDefaultMessage());
            }
            return "validator/index";
        }
        model.addAttribute("msg", user.toString());
        return "validator/success";
    }

    @RequestMapping("/login2")
    public String login2(Model model, User user) {
        try {
            model.addAttribute("msg", this.validatorService.checkUser(user.getUsername(), user.getPassword()));
        } catch (ConstraintViolationException e) {
            for (ConstraintViolation constraintViolation : e.getConstraintViolations()) {
                String key = ((PathImpl) constraintViolation.getPropertyPath()).getLeafNode().getName() + "Valid";
                model.addAttribute(key, constraintViolation.getMessage());
            }
            return "validator/index";
        }
        return "validator/success";
    }

}
```

这里使用 `fieldError.getField() + "Valid"` 组成统一格式的校验反馈。login 实现了实例化的属性校验，login2 通过 validatorService 完成了方法的校验。

### 3.5.3 业务层

```java
package com.springmvc.service;

import org.springframework.stereotype.Service;
import org.springframework.validation.annotation.Validated;

import javax.validation.ConstraintViolationException;
import javax.validation.constraints.NotEmpty;
import javax.validation.constraints.Size;

/**
 * ValidatorService.java
 * <p>
 * Created by 阳君 on 2017/10/16.
 * Copyright © 2017年 springmvc. All rights reserved.
 */
@Validated
@Service
public class ValidatorService {

    public String checkUser(@NotEmpty(message = "{username.NotEmpty}")
                                    String username,
                            @NotEmpty(message = "{password.NotEmpty}")
                            @Size(min = 6, max = 9, message = "{password.Size}")
                                    String password) throws ConstraintViolationException {
        return "User [username=" + username + ", password=" + password + "]";
    }
    
}
```

业务层的 checkUser 方法实现了方法校验，当校验不通过时会报 ConstraintViolationException 错误。

## 3.5 效果图

运行项目，点登录，即可看见如下效果。

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017101602.png)

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Related Documentation

[spring-framework-reference](https://docs.spring.io/spring/docs/5.0.0.RELEASE/spring-framework-reference/core.html#validation-beanvalidation)

[Bean Validation 2.0](http://beanvalidation.org/2.0/spec/#builtinconstraints)

[Validator 6.0.2.Final](http://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/#validator-defineconstraints-hv-constraints)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-10-17 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974