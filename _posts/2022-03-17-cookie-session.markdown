---
layout: post
title:  " Cookie、Session、JSP "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-17 22:56:26 +0800
tags: [Javaweb, Cookie, Session,JSP]
---

#### 保存会话的两种技术：cookie 、session (我们可以将信息或者数据保存在Session中)

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    // 服务器告知访问的时间，将这个时间封装为一个信件
    PrintWriter out = resp.getWriter();

    //Cookie:服务器端从客户端获取
    Cookie[] cookies = req.getCookies();
    // cookie 可能存在多个

    // 判断cookie是否存在
    if(cookies!=null) {
        // 如果存在怎么办
        out.write("你上一次访问的时间为：");
        for (int i = 0; i < cookies.length; i++) {
            Cookie cookie = cookies[i];
            if (cookie.getName().equals("lastLoginTime")) {
                // 获取cookie中的值
                long lastLoginTime = Long.parseLong(cookie.getValue());
                Date date = new Date(lastLoginTime);
                out.write(date.toLocaleString());
            }
        }
    }else {
        out.write("这是你第一次访问本站~");
    }

    // 服务器给客户端响应一个cookie
    Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");
    resp.addCookie(cookie);

    // 设置cookie有效期
    cookie.setMaxAge(24*60*60);

    // 解决中文乱码问题
    req.setCharacterEncoding("UTF-8");
    resp.setCharacterEncoding("UTF-8");
    resp.setHeader("Content-type","text/html;charset=UTF-8");
}
```

cookie：一般会保存在本地的用户目录下 appdata;

#### 一个网站cookie是否存在上限

-  一个cookie只能保存一个信息
- 一个web站点可以给浏览器发送多个cookie，最多存放20个cookie
- cookie大小限制为4kb
- 300个cookie浏览器上限

#### 删除cookie:

- 不设置有效期，关闭浏览器，自动失效
- 设置有效期时间为0



## Session（重点）

- 服务器会给每个用户（浏览器）创建一个session对象
- 一个Session独占一个浏览器，只有浏览器没有关闭，这个session就存在
- 用户登录之后，整个网站都可以访问

#### Session和Cookie的区别：

- Cookie是把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）
- Session把用户的数据写到用户独占的Session中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
- Session对象由服务创建

#### 使用场景：

- 保存一个登录的信息
- 购物车信息
- 在整个网站中会经常使用的数据，我们将他保存在Session中

#### 会话自动过期：

```java
<session-config>
    <session-timeout>15</session-timeout>
</session-config>
```



## JSP:Java Server Pages

Java服务器端页面，也和Servlet一样，用于动态web技术

最大的特点：

-  写JSP就像在写HTML
  - HTML只给用户提供静态的数据
  - JSP页面中可以嵌入JAVA代码，为用户提供动态数据

JSP本质上就是一个Servlet:

```java
public void _jspInit() {
}// 初始化

public void _jspDestroy() {
}// 销毁

public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)
    throws java.io.IOException, javax.servlet.ServletException {// JSPService
```

1. 判断请求

2. 内置一些对象

   ```java
   final javax.servlet.jsp.PageContext pageContext;  // 页面上下文
   javax.servlet.http.HttpSession session = null; // Session
   final javax.servlet.ServletContext application;// applicationContext
   final javax.servlet.ServletConfig config;// config
   javax.servlet.jsp.JspWriter out = null;// out
   final java.lang.Object page = this; // page：当前页
   final javax.servlet.http.HttpServletRequest request  // 请求
   final javax.servlet.http.HttpServletResponse response // 响应
   ```

3. 输出页面前增加的代码

   ```java
   response.setContentType("text/html"); // 设置响应的页面类型
   pageContext = _jspxFactory.getPageContext(this, request, response,
                                             null, true, 8192, true);
   _jspx_page_context = pageContext;
   application = pageContext.getServletContext();
   config = pageContext.getServletConfig();
   session = pageContext.getSession();
   out = pageContext.getOut();
   _jspx_out = out;
   ```

4. 以上的这些对象我们可以在JSP的页面中直接使用



### JSP的基础语法：

JSP作为java技术的一种应用，Java的所有语法都支持

#### JSP表达式：

```jsp
<%--  JSP表达式作用是将程序的输出到客户端--%>
<%=new java.util.Date()%>
```

#### JSP脚本片段：

```jsp
<%
int sum = 0;
for (int i = 0; i <= 100; i++) {
    sum += i;
}
out.println("<h1>SUM="+sum+"</h1>");
%>
```



#### 脚本片段的再实现：

```jsp
<%--  在代码中嵌入HTML元素--%>
  <%
    for (int i = 0; i < 5; i++) {
  %>
      <h1>Hello,world ,number is <%=i%></h1>
  <%
      }
  %>
```



#### JSP声明：

```jsp
<%!
    static {
    System.out.println("loading Servlet!");
}
private int globalVar = 0;
public void belle(){
    System.out.println("进入方法belle");
}
%>
```

JSP声明会被编译到JSP生成的Java类种，其他的则会被生成到JSPService方法中





### JSP指令：

```jsp
<%@ page errorPage="/error/500.jsp" %>
```

### JSP标签：

```jsp
<jsp:include page="header.jsp" />
<h1>网页主题</h1>
<jsp:include page="footer.jsp" />
```



