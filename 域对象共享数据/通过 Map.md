---
up:
  - "[[Spring MVC 課程描述]]"
---
> **食用方式**：形式与`Model`方式类似

后台测试代码

```java
@RequestMapping("/testRequestByMap")
public String testRequestByMap(Map<String, Object> map) {
    //向请求域共享数据
    map.put("testRequestScope", "hello, Map!");
    return "successrequest";
}
```

前台测试代码

```html
<a th:href="@{/scopeController/testRequestByMap}">通过 Map</a><br/>
```

测试结果

![[1755431156546_Z11ZubN4eG.gif]]