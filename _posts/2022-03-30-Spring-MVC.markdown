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



- 返回对象或集合

