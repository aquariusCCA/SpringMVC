---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
  - "[[@ResponseBody]]"
---
后台测试代码

```java
@GetMapping("/testResponseBody")
@ResponseBody
public String testResponseBody() throws IOException {
    return "success";
}
```

前台测试代码

```html
<a th:href="@{/httpController/testResponseBody}">通过 @ResponseBody 响应浏览器数据</a>
```

测试结果

![[1755507292878_CYUT7kAyZj.gif]]