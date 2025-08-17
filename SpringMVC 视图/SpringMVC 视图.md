---
up:
  - "[[Spring MVC 課程描述]]"
---
SpringMVC 中的视图是`View`接口，视图的作用渲染数据，将模型`Model`中的数据展示给用户

SpringMVC 视图的种类很多，默认有 **[[转发视图]]** `InternalResourceView`和 **[[重定向视图]]** `RedirectView`

当工程引入`jstl`的依赖，转发视图会自动转换为`JstlView`（JSP 内容了解即可）

若使用的视图技术为`Thymeleaf`，在 SpringMVC 的配置文件中配置了`Thymeleaf`的视图解析器，由此视图解析器解析之后所得到的是 [[ThymeleafView]]

> **注意**：只有在视图名称没有任何前缀时，视图被`Thymeleaf`视图解析器解析之后，创建的才是[[ThymeleafView]]。当视图名称包含前缀（如`forward:`或`redirect:`）时，分别对应的时`InternalResourceView` [[转发视图]] 和 `RedirectView` [[重定向视图]]