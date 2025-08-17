---
up:
  - "[[Spring MVC 課程描述]]"
---
> **食用方式**：形式与`HttpServletRequest`方式类似，形参为`HttpSession`。需要注意的是 SpringMVC 虽然提供了一个`@SessionAttribute`注解，但并不好用，因此反而建议直接使用原生 Servlet 中的`HttpSession`对象

后台测试代码

```java
@RequestMapping("/testSession")
public String testSession(HttpSession session) {
    //向session域共享数据
    session.setAttribute("testSessionScope", "hello, HttpSession!");
    return "successsession";
}
```

前台测试代码

`index.html`

```html
<a th:href="@{/scopeController/testSession}">通过 Servlet API 向 Session 域对象共享数据</a><br/>
```

`successsession.html`

```html
<p th:text="${session.testSessionScope}"></p>
```

测试结果

![[1755431156554_XAZgznS00r.gif]]