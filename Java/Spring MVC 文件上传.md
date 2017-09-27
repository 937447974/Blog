Spring MVC 上传文件，主要是使用表单上传。这里不再过多描述，相关项目搭建也不在过多描述。

# 1 前端代码

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>文件上传</title>
    <script type="text/javascript" src=${pageContext.request.contextPath}/js/jquery-1.11.3.min.js></script>
    <script type="text/javascript">
        function check() {
            var file = document.getElementById("file").value;
            if (file.length == 0 || file == "") {
                alert("请选择上传文件");
                return false;
            }
            return true;
        }

        function fileUpload() {
            if (check()) {
                // FormData 对象
                var form = new FormData($("#uploadForm")[0]);
                // XMLHttpRequest 对象
                var xhr = new XMLHttpRequest();
                xhr.open("post", "${pageContext.request.contextPath }/fileUpload", true);
                xhr.upload.addEventListener("progress", progressFunction, false);
                xhr.send(form);
            }
        }

        function progressFunction(evt) {
            if (evt.lengthComputable) {
                var progressBar = document.getElementById("progressBar");
                progressBar.max = evt.total;
                progressBar.value = evt.loaded;
                if (evt.loaded == evt.total) {
                    alert("上传完成100%" + evt.total);
                }
            }
        }

    </script>
</head>
<body>
<%-- post 提交
<form action="${pageContext.request.contextPath }/fileUpload" method="post" enctype="multipart/form-data"
      onsubmit="return check()">
    请选择文件：<input id="file" type="file" name="uploadfile" multiple="multiple"/><br/>
    <input type="submit" value="上传"/>
</form>
--%>
<%--ajax 提交--%>
<form id="uploadForm">
    请选择文件：<input id="file" type="file" name="uploadfile" multiple="multiple"/><br/>
    <progress id="progressBar" value="10" max="100"></progress>
    <br/>
    <input type="button" value="上传" onclick="fileUpload()"/>
</form>
</body>
</html>
```

前端可以使用 post 和 ajax 提交，如果想监听上传进度，需要使用 ajax 提交表单。

# 2 Spring MVC 配置

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
       http://www.springframework.org/schema/mvc/spring-mvc-4.3.xsd">

    <!-- 配置视图解释器ViewResolver -->
    <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

    <!-- 配置扫描器 -->
    <context:component-scan base-package="com.upload.controller"/>
    <!--配置注解驱动  -->
    <mvc:annotation-driven/>

    <!--配置静态资源的访问映射，此配置中的文件，将不被前端控制器拦截 -->
    <mvc:resources location="/js/" mapping="/js/**"/>

    <!-- 配置文件上传解析器 MultipartResolver -->
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">      
        <property name="defaultEncoding" value="UTF-8"/>
        <property name="maxUploadSize" value="104857600"/>
    </bean>

</beans>
```

Spring MVC 为上传文件提供了直接的支持，主要是通过 MultipartResolver 接口对象实现的，这里我们配置了自带实现类 CommonsMultipartResolver。如果想监听内部更详细的过程，可以重写 CommonsMultipartResolver。

# 3 控制器

```java
package com.upload.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.multipart.MultipartFile;

import javax.servlet.http.HttpServletRequest;
import java.io.File;
import java.util.List;

@Controller
public class FileUploadController {

    @RequestMapping(value = "/fileUpload")
    public String handleFormUpload(@RequestParam("uploadfile") List<MultipartFile> uploadfile, HttpServletRequest request) {
        // http://www.plupload.com
        if (!uploadfile.isEmpty()) {
            for (MultipartFile file : uploadfile) {
                // 获取上传文件的原始名称
                String originalFilename = file.getOriginalFilename();
                // 设置上传文件的保存地址目录
                String dirPath = request.getServletContext().getRealPath("/upload/");
                File filePath = new File(dirPath);
                // 如果保存文件的地址不存在，就先创建目录
                if (!filePath.exists()) {
                    filePath.mkdirs();
                }
                String filePathName = dirPath + originalFilename;
                try {
                    // 使用MultipartFile接口的方法完成文件上传到指定位置
                    file.transferTo(new File(filePathName));
                    System.out.println(filePath);
                } catch (Exception e) {
                    e.printStackTrace();
                    return "error";
                }
            }
            return "success";
        } else {
            return "error";
        }
    }

}

```

# 4 效果图

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2017092701.png)

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-09-27 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974