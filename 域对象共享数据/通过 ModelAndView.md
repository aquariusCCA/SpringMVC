---
up:
  - "[[Spring MVC 課程描述]]"
---
> **食用方式**：在 SpringMVC 中，不管用的何种方式，本质上最后都会封装到 `ModelAndView`。同时要注意使用 `ModelAndView` 向 request 域对象共享数据时，需要返回`ModelAndView`自身

后台测试代码

```java
@RequestMapping("/testRequestByModelAndView")
public ModelAndView testRequestByModelAndView() {
    /**
     * ModelAndView有Model和View两个功能
     * Model用于向请求域共享数据
     * View用于设置视图，实现页面跳转
     */
    ModelAndView mv = new ModelAndView();
    //向请求域共享数据
    mv.addObject("testRequestScope", "hello, ModelAndView!");
    //设置视图，实现页面跳转
    mv.setViewName("successrequest");
    return mv;
}
```

前台测试代码

```html
<a th:href="@{/scopeController/testRequestByModelAndView}">通过 ModelAndView</a><br/>
```

测试结果

![[1755429743140_czg2MM17gg.gif]]