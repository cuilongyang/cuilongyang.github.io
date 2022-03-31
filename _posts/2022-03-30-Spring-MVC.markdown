---
layout: post
title:  "SpringMVC数据响应"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-30 10:23:01 +0800
tags: [Java, SpringMVC]
---

## SpringMVC的数据响应方式

### 1. 页面跳转

#### 1.1直接返回字符串

直接返回字符串：此种方式会将返回的字符串与视图解析器的前后缀拼接后跳转。

以下为UserController代码：

```java
@RequestMapping(value = "/quick" ,method = RequestMethod.GET)
public String hi(){
    System.out.println("controller hi running...");
    // 此处添加了内部资源视图解析器
    return "success";
}
```

以下为spring-mvc.xml的配置

```xml
<!--    配置内部资源视图解析器-->
<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

二者结合：**转发**资源地址为：`/WEB-INF/jsp/success.jsp`  

**重定向**：`redirect:/success.jsp`

#### 1.2  通过ModelAndView对象返回

```java
@RequestMapping(value = "/hello")
public ModelAndView hello(){
    ModelAndView modelAndView = new ModelAndView();
    /*   Model模型：作用是封装数据
         View视图：作用展示数据    */
    // 设置视图名称
    modelAndView.setViewName("index");
    // 设置模型数据
    modelAndView.addObject("username","belle");
    return modelAndView;
}
```

或者如下：

```java
@RequestMapping(value = "/hi")
public ModelAndView hi(ModelAndView modelAndView){
    modelAndView.setViewName("index");
    modelAndView.addObject("username","clyde");
    return modelAndView;
}
```



我们在jsp页面中通过`${}` 方式获取username对应的值

```jsp
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
    <h1>Hello,world</h1>
    <h4>From web/jsp/index.jsp</h4>
    <h1>${username}</h1>
  </body>
</html>
```



### 2.回写数据

#### 2.1直接返回字符串

客户端访问服务器端，如果想直接回写字符串作为响应体返回的话，只需要使用

`response.getWriter().print(“hello world”)` 即可，在Controller中采取如下办法

- 通过SpringMVC框架注入的response对象，使用response.getWriter().print(“hello world”) 回写数据，此时不需要

  视图跳转，业务方法返回值为void。

  ```java
  @RequestMapping(value = "/five")
  public void five(HttpServletResponse response) throws IOException {
      response.getWriter().println("hello,world");
  }
  ```

- 将需要回写的字符串直接返回，但此时需要通过`@ResponseBody`注解告知SpringMVC框架，方法返回的字符

  串**不是跳转**是直接在http响应体中返回。**（重点！！！）**

  ```java
  @RequestMapping(value = "/seven")
  @ResponseBody  // 告知SpringMVC不进行视图跳转，直接进行数据响应
  public String seven() throws IOException {
      return "hello,Spring";
  }
  ```


#### 2.2返回对象或集合

  在异步项目中，客户端与服务器端往往要进行json格式字符串交互，此时我们可以手动拼接json字符串返回。

  ```java
  @RequestMapping(value = "/eight")
  @ResponseBody
  public String eight() throws IOException {
  return "{\"username\":\"belle\",\"age\":18}";
  }
  ```

  以上的办法，在数据量大时，拼接效率及其低下，我们采用第三方工具：

  ```xml
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.9.0</version>
  </dependency>
  <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-annotations</artifactId>
      <version>2.9.0</version>
  </dependency>
  ```

  ```java
  @RequestMapping(value = "/nine")
  @ResponseBody
  public String nine() throws IOException {
      User user = new User();
      user.setUsername("clyde");
      user.setAge(18);
      // 使用json转换工具，将对象转换为json格式的字符串在返回
      ObjectMapper objectMapper = new ObjectMapper();
      String json = objectMapper.writeValueAsString(user);
      return json;
  }
  ```

  **MVC注解驱动**

  以上的办法需要引入json转换工具，我们可以让SpringMVC来做这些工作

  在方法上添加@ResponseBody就可以返回json格式的字符串，但是这样配置比较麻烦，配置

  代码比较多，因此，我们可以使用mvc的注解驱动代替上述配置。

  ```xml
  <!--    mvc注解驱动-->
  <mvc:annotation-driven/>
  ```

  

## SpringMVC获得请求数据

客户端请求参数的格式是：`name=value&name=value… …`

服务器端要获得请求的参数，有时还需要进行数据的封装，SpringMVC可以接收如下类型的参数：

- 基本类型参数
- POJO类型参数
- 数组类型参数
- 集合类型参数

### 1.获得基本类型参数

Controller中的业务方法的参数名称要与请求参数的name一致，参数值会自动映射匹配。

```java
@RequestMapping(value = "/eleven")
@ResponseBody
public void eleven(String username, int age) throws IOException {
    System.out.println(username);
    System.out.println(age);
}
```

浏览器输入：`http://localhost:8080/eleven?username=belle&age=18`

控制台会输出 `belle  18`

### 2.获得POJO类型参数

Controller中的业务方法的POJO参数的属性名与请求参数的name一致，参数值会自动映射匹配。

```java
@RequestMapping(value = "/twelve")
@ResponseBody
public void twelve(User user) throws IOException {
    System.out.println(user);
}
```

浏览器输入：`http://localhost:8080/twelve?username=belle&age=18`

控制台输出：`User{username='belle', age=18}`

### 3.获得数组类型参数

Controller中的业务方法数组名称与请求参数的name一致，参数值会自动映射匹配。

```java
@RequestMapping(value = "/thirteen")
@ResponseBody
public void thirteen(String[] strings) throws IOException {
    System.out.println(Arrays.asList(strings));
}
```

浏览器输入：`http://localhost:8080/thirteen?strings=111&strings=222&strings=333`

控制台输出：`[111, 222, 333]`

### 4.获得集合类型参数

获得集合参数时，要将集合参数包装到一个POJO中才可以。

```java
@RequestMapping(value = "/fourteen")
@ResponseBody
public void fourteen(VO vo) throws IOException {
    System.out.println(vo);
}
```

以上的VO对象为：

```java
public class VO {
    private List<User> userList;
```

```jsp
<body>
    <form action="${pageContext.request.contextPath}/fourteen" method="post">
        <%--  表明时第几个User对象的username age--%>
        <input type="text" name="userList[0].username"><br/>
        <input type="text" name="userList[0].age"><br/>
        <input type="text" name="userList[1].username"><br/>
        <input type="text" name="userList[1].age"><br/>
        <input type="submit" value="提交">
    </form>
</body>
```

以上列表输入后：控制台打印：

`VO{userList=[User{username='belle', age=12}, User{username='clyde', age=55}]}`





