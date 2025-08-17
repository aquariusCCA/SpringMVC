---
up:
  - "[[Spring MVC 課程描述]]"
---
后台测试代码

```java
@RequestMapping("/testRequestByServletAPI")
public String testRequestByServletAPI(HttpServletRequest request) {
    request.setAttribute("testRequestScope", "hello, Servlet API!");
    return "successrequest";
}
```

前台测试代码

`index.html`

```html
<a th:href="@{/scopeController/testRequestByServletAPI}">通过Servlet API</a>
```

`successrequest.html`

```html
<p th:text="${testRequestScope}"></p>
```

测试结果

![[1755429743140_ZqDPUoCPgN.gif]]

可以发现，转发的页面中成功获取到了在后台通过`Request`对象向`request`域中设置的属性值并正确展示