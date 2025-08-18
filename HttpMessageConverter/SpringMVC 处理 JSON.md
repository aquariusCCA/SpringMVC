---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
  - "[[@ResponseBody]]"
---
处理方式：引入`jackson`依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.13.2</version>
</dependency>
```

这里不修改任何代码，直接重新部署项目，再来测试下

![[1755508001247_yddq8LAaIs.gif]]

这里需要强调的是，并不是只引入`jackson`依赖就够了。实际上，`@ResponseBody`处理`json`的步骤如下

- 1）引入`jackson`依赖
- 2）开启 MVC 注解驱动
    - 此时在`HandlerAdaptor`中会自动装配一个消息转换器`MappingJackson2HttpMessageConverter`，可以将响应到浏览器的 Java 对象转换`json`格式的字符串
- 3）使用`@ResponseBody`注解标识控制器方法
- 4）将 Java 对象作为控制器方法的返回值返回
	- 这里会自动转换为`json`类型的==字符串==

上述步骤缺一不可，少一步都实现不了效果

---

# **回顾**：

我们之前章节中就说过 MVC 注解驱动的功能很多，在  _SpringMVC 视图_ 中处理了 [[视图控制器 view-controller|视图控制器]] 问题，在  _RESTful 案例_ 中处理静态资源问题，而在本章节中处理了 Java 对象转换为 JSON 对象的问题。

这里再总结一下 MVC 注解驱动的作用：
 
1、解决视图控制器`view-controller`造成其他请求失效的问题

2、解决默认 Servlet 处理器`default-servlet-handler`造成`DispatcherServlet`失效的问题

3、解决 Java 对象转换为 Json 对象的问题

---

# **扩展**：Json 对象和 Json 数组

1、Json 对象用`{}`包裹，如`{"username":"admin","password":"123456"}`

2、Json 数组用`[]`包裹，如`["username":"admin","password":"123456"]`

3、Json 对象和 Json 数组可以相互嵌套，即
`
Json 对象中可以包含 Json 对象和 Json 数组，如

```json
[
	"username":"admin",
	"password":"123456", 
	"ext" : {"age": 18, "gender": "1"}
]

[
	"username":"admin",
	"password":"123456", 
	"ext" : [
		"age": 18, 
		"gender": "1"
	]
]
```

Json 数组中也可以包含 Json 对象和 Json 数组，如

```json
{
	"username":"admin",
	"password":"123456", 
	"ext" : {
		"age": 18, 
		"gender": "1"
	}
}

{
	"username":"admin",
	"password":"123456", 
	"ext" : [
		"age": 18, 
		"gender": "1"
	]
}
```