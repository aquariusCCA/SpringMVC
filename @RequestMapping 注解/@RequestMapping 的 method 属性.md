---
up:
  - "[[Spring MVC 課程描述]]"
---
`@RequestMapping`注解的`method`属性有哪些用途呢？

# 通过请求的请求方式（`GET`或`POST`）匹配请求映射

###  常用的请求方式有 4 种：GET、POST、PUT、DELETE

- GET 用于查询数据
- POST 用于添加数据
- PUT 用于更新数据
- DELETE 用户删除数据

> 但是很多情况下，习惯上会让 POST 承担更多的职责，即通过 POST 进行增、删、改的操作，可以说是 “一个人揽了三个人的活”

### 还有 4 种不常用的请求：HEAD、PATCH、OPTIONS、TRACE

- HEAD 获取响应的报头，检查超链接的有效性、网页是否被修改，多用于自动搜索机器人获取网页的标志信息，获取 rss 种子信息，或者传递安全认证信息等
- PATCH 用于更新数据
- OPTIONS 用于测试服务器，解决同源策略和跨域请求问题
- TRACE 用于测试或诊断

> 有的资料还会介绍 CONNECT 请求，好像用于 HTTP 代理的，很少人听过，当然用的更少（我也是刚知道，有懂的求科普）

---

# 是一个`RequestMethod`类型的数组，表示该请求映射能够匹配多种请求方式的请求

源码为证：

```java
RequestMethod[] method() default {};
```

> 同时注意到`method`属性默认值为空数组，是否说明控制器方法不添加`method`属性时，不同的请求方法都能够匹配呢 ？