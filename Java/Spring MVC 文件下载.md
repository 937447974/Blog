文件下载就是将文件服务器中的文件下载到本机上。在 Spring MVC中，实现文件下载大致分为两步。

# 1 前端

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@page import="java.net.URLEncoder" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
    <title>下载页面</title>
</head>
<body>
<a href="${pageContext.request.contextPath }/download?fileName=<%=URLEncoder.encode("spring开发文档.pdf", "UTF-8")%>">
    中文名称文件下载
</a><br/>
<a href="${pageContext.request.contextPath }/download1?fileName=<%=URLEncoder.encode("spring开发文档.pdf", "UTF-8")%>">
    断点下载
</a>
</body>
</html>
```

这里使用 `URLEncoder.encode` 将中文名转换了，防止中文传输问题。

# 2 后台

后台下载可以有两种方案，一种是使用 spring mvc 自带的 ResponseEnity 对象；一种是使用 ServletOutputStream 输出。

**1 ResponseEnity 下载**

```java
@RequestMapping("/download")
public ResponseEntity<byte[]> fileDownload(HttpServletRequest request, String fileName) throws Exception {
    // 指定要下载的文件所在路径
    String path = request.getServletContext().getRealPath("/upload/");
    // 创建该文件对象
    File file = new File(path + File.separator + fileName);
    // 对文件名编码，防止中文文件乱码
    fileName = this.getFilename(request, fileName);
    // 设置响应头
    HttpHeaders headers = new HttpHeaders();
    // 通知浏览器以下载的方式打开文件
    headers.setContentDispositionFormData("attachment", fileName);
    // 定义以流的形式下载返回文件数据
    headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
    // 使用Sring MVC框架的ResponseEntity对象封装返回下载数据
    return new ResponseEntity<byte[]>(FileUtils.readFileToByteArray(file), headers, HttpStatus.OK);
}

/**
 * 根据浏览器的不同进行编码设置，返回编码后的文件名
 */
String getFilename(HttpServletRequest request, String filename) throws Exception {
    // IE不同版本User-Agent中出现的关键词
    String[] IEBrowserKeyWords = {"MSIE", "Trident", "Edge"};
    // 获取请求头代理信息
    String userAgent = request.getHeader("User-Agent");
    for (String keyWord : IEBrowserKeyWords) {
        if (userAgent.contains(keyWord)) {
            //IE内核浏览器，统一为UTF-8编码显示
            return URLEncoder.encode(filename, "UTF-8");
        }
    }
    //火狐等其它浏览器统一为ISO-8859-1编码显示
    return new String(filename.getBytes("UTF-8"), "ISO-8859-1");
}

```

使用 ResponseEntity，实现简单。一般的文件都可以使用它来完成。这里的中文乱码问题，我们使用 getFilename 方法解决。

**2 ServletOutputStream 下载**

```java
@RequestMapping("/download1")
public ResponseEntity<String> downFile(HttpServletResponse response, HttpServletRequest request, String fileName)
        throws Exception {
    String path = request.getServletContext().getRealPath("/upload/");
    // 创建该文件对象
    File file = new File(path + File.separator + fileName);
    long fSize = file.length();
    response.setCharacterEncoding("utf-8");
    response.setContentType("application/x-download");
    response.setHeader("Accept-Ranges", "bytes");
    response.setHeader("Content-Length", String.valueOf(fSize));
    response.setHeader("Content-Disposition", "attachment;fileName=" + this.getFilename(request, fileName));
    long pos = 0;
    if (null != request.getHeader("Range")) {
        // 断点续传
        response.setStatus(HttpServletResponse.SC_PARTIAL_CONTENT);
        try {
            pos = Long.parseLong(request.getHeader("Range").replaceAll("bytes=", "").replaceAll("-", ""));
        } catch (NumberFormatException e) {
            pos = 0;
        }
    }
    String contentRange = "bytes " + pos + "-" + (fSize - 1) + "/" + fSize;
    response.setHeader("Content-Range", contentRange);
    InputStream inputStream = null;
    ServletOutputStream out = null;
    try {
        inputStream = new FileInputStream(file);
        inputStream.skip(pos);
        out = response.getOutputStream();
        byte[] buffer = new byte[1024 * 100];
        int length = 0;
        while ((length = inputStream.read(buffer, 0, buffer.length)) != -1) {
            out.write(buffer, 0, length);
            // Thread.sleep(100);
        }
    } finally {
        if (null != out) out.flush();
        if (null != out) out.close();
        if (null != inputStream) inputStream.close();
    }
    return new ResponseEntity(null, HttpStatus.OK);
}
```

使用 ServletOutputStream 稍微复杂一点。但是也有好处，我们可以更加精细的控制传输的过程，还能实现断点续传。如果响应头 Content-Length 无法设置，这是因为浏览器请求的时候默认使用了长连接，需要在服务器配置的时候关闭长连接的支持。

&#160;

----------

# Appendix

## Sample Code

[Java](https://github.com/937447974/Java)

## Revision History

| 时间 | 描述 |
| ---- | ---- |
| 2017-09-28 | 博文完成 |

## Copyright

CSDN：http://blog.csdn.net/y550918116j

GitHub：https://github.com/937447974