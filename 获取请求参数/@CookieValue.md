---
up:
  - "[[Spring MVC 課程描述]]"
---
`@CookieValue`是==将 Cookie 数据和控制器方法的形参创建映射关系==

一共有三个属性：`value`、`required`、`defaultValue`，用法同`@RequestParam`

> [!NOTE] **注意**：
> 
> - 在`JSP`中，`Session`依赖于`Cookie`，`Session`是服务器端的会话技术，`Cookie`是客户端的会话技术。
> - 会话技术默认的生命周期是浏览器开启和浏览器关闭，只要浏览器不关闭，`Cookie`将一直存在。
> - 调用`getSession()`方法时，首先会检测请求报文中是否有携带`JSESSIONID`的`Cookie`。如果没有，说明当前会话是第一次创建`Session`对象，则
> 	- 在服务端创建一个`Cookie`，以键值对形式存储。键是固定的`JSESSIONID`，值是一个 UUID 随机序列
> 	- 在服务端创建一个`HttpSession`对象，并放在服务器所维护的 Map 集合中。Map 的键是`JSESSIONID`的值，值就是`HttpSession`对象
> 	- 最后把`Cookie`相应给浏览器客户端，此时`JSESSIONID`的`Cookie`存在于响应报文中。每次浏览器向服务器发送请求都会携带`Cookie`，此后`JSESSIONID`的`Cookie`将存在于请求报文中

为了能获取到`Cookie`值，需要先调用下`getSession()`方法。我们直接在之前的 testServletAPI() 方法中稍作修改

```java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request) {
    HttpSession session = request.getSession();
    // ...
}
```

首次发送请求后，F12 查看前台该请求的 _响应报文_ 信息

![[1755429045106_2hjb6rGidz.png]]

会发现在`Set-Cookie`属性中存在`JSESSIONID=xxx`的信息

```shell
Set-Cookie: JSESSIONID=C3DFF845C38BF655C02DDA0BD2DD5638; Path=/SpringMVC; HttpOnly
```

后面每次发送请求，`JSESSIONID`的`Cookie`将会放在 _请求报文_ 信息

![[1755429045113_aPbIdfuKxE.png]]

会发现在`Cookie`属性中存在`JSESSIONID=xxx`的信息

```shell
Cookie: JSESSIONID=C3DFF845C38BF655C02DDA0BD2DD5638
```

经过上面的折腾，我们产生了`Cookie`数据，现在我们就可以使用`@CookieValue`注解进行操作了。正片开始~

后台测试代码

```java
@RequestMapping("/testCookie")
public String testCookie(
    @CookieValue(value = "JSESSIONID") String jSessionId,
    @CookieValue(value = "Test", required = false, defaultValue = "CookieValue") String test) {
    System.out.println("jSessionId=" + jSessionId + ", test=" + test);
    return "success";
}
```

前台请求报文信息

![[1755429045113_bQ0nzbKTsG.png]]

后台日志信息

```shell
jSessionId=C3DFF845C38BF655C02DDA0BD2DD5638, test=CookieValue
```