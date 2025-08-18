---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
  - "[[@ResponseBody]]"
---
后台测试代码

```java
@GetMapping("/testResponseByServletAPI")
public void testResponseByServletAPI(HttpServletResponse response) throws IOException {
    response.getWriter().print("hello, response");
}
```

前台测试代码

```html
<a th:href="@{/httpController/testResponseByServletAPI}">通过 HttpServletResponse 响应浏览器数据</a>
```

测试结果

![[1755507292878_nyNrjbFY91.gif]]