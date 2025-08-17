---
up:
  - "[[Spring MVC 課程描述]]"
---
`@RequestHeader`是 ==将请求头信息和控制器方法的形参创建映射关系==

一共有三个属性：`value`、`required`、`defaultValue`，用法同 `@RequestParam`

因为`@RequestHeader`与`@RequestParam`别无二致，所以这里我们简单测试下效果

后台测试代码

```java
@RequestMapping("/testHeader")
public String testHeader(
    @RequestHeader(value = "Host") String host,
    @RequestHeader(value = "Test", required = false, defaultValue = "RequestHeader") String test) {
    System.out.println("Host=" + host + ", test=" + test);
    return "success";
}
```

请求路径

```ini
http://localhost:8080/SpringMVC/paramController/testParam4
```

后台日志信息

```shell
Host=localhost:8080, test=RequestHeader
```