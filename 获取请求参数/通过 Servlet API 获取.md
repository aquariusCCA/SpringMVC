---
up:
  - "[[Spring MVC 課程描述]]"
---
将`HttpServletRequest`作为控制器方法的形参，此时`HttpServletRequest`类型的参数表示封装了当前请求的请求报文的对象

后台测试代码

```java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request) {
    String username = request.getParameter("username");
    String password = request.getParameter("password");
    System.out.println("username=" + username + ",password=" + password);
    return "success";
}
```

前台测试代码

```html
<a th:href="@{/paramController/testServletAPI(username='admin',password='123456')}">通过 Servlet API 获取</a><br/>
```

测试结果

![[1755426725863_IfOf6tNrcT.gif]]

后台日志信息

```java
username=admin,password=123456
```

> [!NOTE] **为何将`HttpServletRequest request`传入 testServletAPI() 方法中就可以使用？**
> 
> SpringMVC 的 IOC 容器帮我们注入了`HttpServletRequest` 请求对象，同时`DispatherServlet`为我们调用 testServletAPI() 方法时自动给`request`参数赋了值，因此可以在方法形参位置传入请求对象`HttpServletRequest` 就可以直接使用其`getParameter()`方法获取参数

尽管上述 Servlet API 原生方式可以获取请求参数，但是这样做就没有必要了。因为 SpringMVC 中帮我们封装好了更加便捷的方式获取请求参数