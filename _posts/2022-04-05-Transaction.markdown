---
layout: post
title:  " Transaction "
subtitle: 
cover-img: 
thumbnail-img:
share-img: 
date: 2022-04-05 22:56:26 +0800
tags: [Java, Transaction]
---



# 1. 编程式事务控制相关对象

## 1.1 PlatformTransactionManager

`PlatformTransactionManager` 接口是spring 的事务管理器，它里面提供了我们常用的操作事务的方法。

-  `TransactionStatus getTransaction(TransactionDefination defination)`  获取事务的状态信息
- `void commit(TransactionStatus status)`   提交事务
- `void rollback(TransactionStatus status)`   回滚事务

PlatformTransactionManager 是接口类型，不同的Dao 层技术则有不同的实现类.



## 1.2.TransactionDefinition

`TransactionDefinition` 是事务的定义信息对象，里面有如下方法：

- `int getIsolationLevel()`   获得事务的隔离级别
- `int getPropogationBehavior()`         获得事务的传播行为
- `int getTimeout()`               获得超时时间
- `boolean isReadOnly()`             是否只读

###    1.事务隔离级别

设置隔离级别，**可以解决事务并发产生的问题**，如脏读、不可重复读和幻读。

- ISOLATION_DEFAULT       默认
- ISOLATION_READ_UNCOMMITTED      读未提交
- ISOLATION_READ_COMMITTED       读已提交（解决脏读）
- ISOLATION_REPEATABLE_READ    重复读 （解决不可重复读）
- ISOLATION_SERIALIZABLE    串行化（全部解决，但性能较低）

### 2.事务传播行为

解决业务方法在调用业务方法时，事务间统一性的问题。

- **REQUIRED：如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）**
- **SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）**
- MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常
- REQUERS_NEW：新建事务，如果当前在事务中，把当前事务挂起。
- NOT_SUPPORTED：以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
- NEVER：以非事务方式运行，如果当前存在事务，抛出异常
- NESTED：如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行REQUIRED 类似的操作
- 超时时间：默认值是-1，没有超时限制。如果有，以秒为单位进行设置
- 是否只读：建议查询时设置为只读



### 3.TransactionStatus

`TransactionStatus` 接口提供的是事务具体的运行状态，方法介绍如下：

- `boolean hasSavepoint()`      是否存储回滚点

- `boolean isCompleted()`      事务是否完成
- `boolean isNewTransaction()`       是否是新事务
- `boolean isRollbackOnly()`          事务是否回滚

