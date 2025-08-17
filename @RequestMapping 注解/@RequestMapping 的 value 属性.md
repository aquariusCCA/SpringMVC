---
up:
  - "[[Spring MVC 課程描述]]"
---
`@RequestMapping`注解的`value`属性有哪些用途呢 ？

- **请求映射**：通过请求的请求地址匹配请求映射
- **匹配多个请求**：是一个字符串类型的数组，表示该请求映射能够匹配多个请求地址所对应的
- **必须设置**：至少通过一个请求地址匹配请求映射

# 匹配多个请求

查看`@RequestMapping`注解的源码，会发现其`value`属性返回值为 String 类型的数组，这也说明了之所以`@RequestMapping`注解的`value`属性可以匹配多个请求的原因。通过为`value`属性指定多个值的方式，就可以对个多个请求建立请求映射

![[1755333753420_wH40tkm4yS.png]]

在控制器方法上的`@RequestMapping`中新增一个`/test`请求映射

```java
@RequestMapping(value = {"/success", "/test"})
public String success() {
    return "success";
}
```

前台创建一个`/test`的请求路径的超链接，以便进行测试

```html
<a th:href="@{/requestMappingController/test}">>测试RequestMapping注解的value属性-->/test</a>
```

测试结果

![[1755422673268_2y3tg2KdqV.gif]]

这样，同一个控制器方法就可以实现对多个请求进行统一处理