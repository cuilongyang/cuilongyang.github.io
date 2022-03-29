---
layout: post
title:  "Spring Annotation"
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-03-28 10:23:01 +0800
tags: [Java, Spring Annotation]
---

## Spring配置数据源

1.1 数据源的开发步骤：







## Spring注解开发：

### 1.1 原始注解

Spring是轻代码而重配置的框架，配置比较繁重，影响开发效率，所以注解开发是一种趋势，**注解代替xml配置**文

件可以简化配置，提高开发效率。

**Spring原始注解主要是替代`<Bean>`的配置**

- **@Component**              使用在**类**上用于实例化Bean  
- **@Controller**                使用在web层类上用于实例化Bean
- **@Service**                     使用在service层类上用于实例化Bean
- **@Repository**               使用在dao层类上用于实例化Bean
- **@Autowired**                使用在字段上用于根据类型依赖注入
- **@Qualifier**                   结合@Autowired一起使用用于根据名称进行依赖注入
- **@Resource**                  相当于@Autowired+@Qualifier，按照名称进行注入
- **@Value**                         注入普通属性
- **@Scope**                        标注Bean的作用范围
- @PostConstruct           使用在方法上标注该方法是Bean的初始化方法
- @PreDestroy                使用在方法上标注该方法是Bean的销毁方法



### 1.2 Spring新注解

原始注解对于非自定义的Bean就无法进行注解，我们无法在jar包上进行注解，所以需要新注解来解决这些问题

- 非自定义的Bean的配置：`<bean>`
- 加载properties文件的配置：`<context:property-placeholder>`
- 组件扫描的配置：`<context:component-scan>`
- 引入其他文件：`<import>`

新注解：

- @Configuration         用于指定当前类是一个Spring 配置类，当创建容器时会从该类上加载注解

- @ComponentScan    用于指定Spring 在初始化容器时要扫描的包。

   作用和在Spring 的xml 配置文件中的`<context:component-scan base-package="com.belle"/>`一样

- @Bean                        使用在方法上，**标注将该方法的返回值存储到Spring 容器中**

- @PropertySource       用于加载.properties 文件中的配置

- @Import                      用于导入其他配置类



