---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[params 属性]]"
---
这里指定`params`的值指定为`username`，这就要求请求中必须携带`username`的请求参数

```java
@RequestMapping(
    value = {"/testParams"},
    params = {"username"}
)
public String testParams() {
    return "success";
}
```

前台测试代码：分别不加请求参数和加上请求参数，进行测试

```html
<a th:href="@{/requestMappingController/testParams}">测试RequestMapping注解的params属性==>testParams</a><br/>
<a th:href="@{/requestMappingController/testParams?username=admin}">测试RequestMapping注解的params属性==>testParams?username=admin</a>
```

测试结果

![[1755424292410_eiYqAXRruw.gif]]

可以发现，当配置了`params`属性并指定相应的请求参数时，请求中必须要携带相应的请求参数信息，否则前台就会报抛出`400`的错误信息，符合预期

```js
HTTP Status 400：Parameter conditions "username" not met for actual request parameters
```

不过在`Tymeleaf`中使用问号的方式会有错误提示，虽然不影响功能，但不想要错误提示的话，最好通过`(...,...)`的方式进行包裹，多个参数间通过`,`隔开

```html
<a th:href="@{/requestMappingController/testParams(username='admin', password=123456)}">测试RequestMapping注解的params属性==>testParams(username='admin', password=123456)</a><br/>
```

测试验证

![[1755424961418_V7kzi2jcsj.gif]]

可以发现，通过括号包裹的方式，`Tymeleaf`最终会帮我们将其解析成`?username=admin&password=123456`的格式

> 存疑点：实测发现，testParams(username='admin', password=123456)`改成`testParams(username=admin, password=123456)`，即`admin 不加单引号也是可以的，这与课堂上所讲的并不一致，此点存疑