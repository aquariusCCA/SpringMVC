---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[SpringMVC 视图]]"
---
本节内容较少，主要掌握

- SpringMVC 中默认的视图：[[转发视图|InternalResourceView]]、[[重定向视图|RedirectView]]
    - 使用`forward:`前缀：[[转发视图|InternalResourceView 视图]]
    - 使用`redirect:`前缀：[[重定向视图|RedirectView 视图]]

- `Thymeleaf`对应 [[ThymeleafView]] 视图（无任何前缀时），`jstl`对应`JstlView`

- 注意 [[转发和重定向的区别]] ：请求次数、浏览器地址栏地址、`request`域对象、访问`WEB-INF`下资源、跨域等方面

- [[InternalResourceViewResolver]] 视图解析器的使用