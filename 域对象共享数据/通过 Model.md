---
up:
  - "[[Spring MVC 課程描述]]"
---
> **食用方式**：形式与`HttpServletRequest`类似

后台测试代码

```java
@RequestMapping("/testRequestByModel")
public String testRequestByModel(Model model) {
    //向请求域共享数据
    model.addAttribute("testRequestScope", "hello, ModelAndView!");
    return "successrequest";
}
```

前台测试代码

```html
<a th:href="@{/scopeController/testRequestByModel}">通过 Model</a><br/>
```

测试结果

![[1755429743140_2bGdk5z3iz.gif]]

