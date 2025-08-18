---
up:
  - "[[Spring MVC 課程描述]]"
---
当前 SpringMVC 配置文件中存在这样几个配置

```xml
<!--配置视图控制器-->
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>

<!--开放静态资源访问-->
<mvc:default-servlet-handler/>

<!--配置MVC注解驱动-->
<mvc:annotation-driven/>
```

各个配置的作用

- [[视图控制器 view-controller|view-controller]]：简化页面跳转实现
- `default-servlet-handler`：开放静态资源访问
- `annotation-driven`：这里主要有 2 个作用
    - 1）解决因配置视图控制器导致其他请求失效（404）的问题
    - 2）解决因配置静态资源访问导致其他请求失效（404）的问题

> **回忆**：在 _SpringMVC 视图_ 、[[视图控制器 view-controller]] 中有介绍过 “MVC 注解驱动” 功能作用

`annotation-driven`需要与`view-controller`、`default-servlet-handler`配合使用

`annotation-driven`与`view-controller`的关系

- 当配置了`view-controller`而不配置`annotation-driven`，那么除了视图控制器中配置的请求，其他控制器方法将无法访问，即其他请求失效（404）
- 当配置了`view-controller`也配置了`annotation-driven`，那么视图控制器中配置的请求和其他控制器方法都能够正常访问

`annotation-driven`与`default-servlet-handler`的关系

- 当配置了`default-servlet-handler`而不配置 `annotation-driven`，那么所有请求都将交给`DefaultServlet`处理，`DispatcherServlet`将失效
- 当配置了`default-servlet-handler`也配置了`annotation-driven`，那么所有请求将先交给`DispatcherServlet`处理，处理不了再交给`DefaultServlet`处理