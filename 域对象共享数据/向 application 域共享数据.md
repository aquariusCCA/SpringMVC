---
up:
  - "[[Spring MVC 課程描述]]"
---
> **食用方式**：形式与`HttpSession`方式类似，只不过需要先从`session`对象中获取`ServletContext`上下文对象，即`application`域对象，再做操作

后台测试代码

```java
@RequestMapping("/testApplication")
public String testApplication(HttpSession session) {
    ServletContext application = session.getServletContext();
    application.setAttribute("testApplicationScope", "hello, application!");
    return "successapplication";
}
```

前台测试代码

`index.html`

```html
<a th:href="@{/scopeController/testApplication}">通过 Servlet API 向 Application 域对象共享数据</a><br/>
```

`successapplication.html`

```html
<p th:text="${application.testApplicationScope}"></p>
```

测试结果

![[1755431563400_clagkCaMGK.gif]]