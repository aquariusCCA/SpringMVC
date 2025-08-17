---
up:
  - "[[Spring MVC 課程描述]]"
---
> **食用方式**：形式与`Model`方式类似

后台测试代码

```java
@RequestMapping("/testRequestByModelMap")
public String testRequestByModelMap(ModelMap modelMap) {
    //向请求域共享数据
    modelMap.addAttribute("testRequestScope", "hello, ModelMap!");
    return "successrequest";
}
```

前台测试代码

```html
<a th:href="@{/scopeController/testRequestByModelMap}">通过 ModelMap</a><br/>
```

测试结果

![[1755431156554_QlONa3aj7n.gif]]