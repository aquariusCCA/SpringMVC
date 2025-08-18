---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
---
`@RequestBody`可以获取请求体，需要在控制器方法设置一个形参，使用`@RequestBody`进行标识，当前请求的请求体就会为当前注解所标识的形参赋值

后台测试代码

```java
@Controller
@RequestMapping("/httpController")
public class HttpController {
    @PostMapping("/testRequestBody")
    public String testRequestBody(@RequestBody String requestBody) {
        System.out.println("requestBody: " + requestBody);
        return "success";
    }
}
```

前台测试代码

```html
<form th:action="@{httpController/testRequestBody}" method="post">
    用户名：<input type="text" name="username"><br/>
    密码：<input type="password" name="password"><br/>
    <input type="submit" value="测试@RequestBody">
</form>
```

测试结果

![[1755504967896_fCHI0GfSO8.gif]]

后台日志信息

```shell
requestBody: username=admin&password=123456
```