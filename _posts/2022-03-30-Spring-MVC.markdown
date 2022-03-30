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







### 2.回写数据

- 直接返回字符串
- 返回对象或集合

