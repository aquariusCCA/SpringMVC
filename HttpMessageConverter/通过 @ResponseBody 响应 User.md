---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
  - "[[@ResponseBody]]"
---
后台测试代码

```java
@GetMapping("/testResponseUser")
@ResponseBody
public User testResponseUser() throws IOException {
    return new User("admin", "123456");
}
```

前台测试代码

```html
<a th:href="@{/httpController/testResponseUser}">通过 @ResponseBody 响应 User</a>
```

测试结果

![[1755507292878_7lpe4lJEbK.gif]]

我这里报了`406 - 不可接收`的错误

> **存疑点**：课程中报的是`500 - No converter found for return value of type: class xxx`，不知道是什么原因，此处存疑
> 
> ![[1755507292878_Q4gfcPuv1O.png]]

**Q**：那么如何解决这个问题？

**A**：不要响应 Java 对象，而是[[SpringMVC 处理 JSON|转换成 Json 对象]]