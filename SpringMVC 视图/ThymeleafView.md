---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[SpringMVC 视图]]"
---
当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被 SpringMVC 配置文件中所配置的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过转发的方式实现跳转

后台测试代码

```java
@RequestMapping("/testThymeleaftView")
public String testThymeleaftView() {
    return "success";
}
```

前台测试代码

```html
<a th:href="@{/viewController/testThymeleaftView}">测试 ThymeleaftView</a><br/>
```

断点调试，查看创建的`View`视图对象为`ThymeleafView`对象

![[1755431563406_32MqQGn7sY.png]]