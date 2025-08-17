---
up:
  - "[[Spring MVC 課程描述]]"
---
- 功能：==将请求和处理请求的控制器方法关联起来，建立映射关系==

- 位置：作用在类上（请求路径的初始信息）；作用在方法上（请求路径的具体信息）

- [[@RequestMapping 的 value 属性 | value 属性]] ：可以匹配多个请求路径，匹配失败报`404`

- [[@RequestMapping 的 method 属性 | method 属性]] ：支持`GET`、`POST`、`PUT`、`DELETE`，默认不限制，匹配失败报`405`

- [[params 属性]]：四种方式，[[param]]、[[!param]]、[[param=value]]、[[param!=value]]，匹配失败报`400`

- [[headers 属性]]：四种方式，`header`、`!header`、`header==value`、`header!=value`，匹配失败报`400`

- 支持 [[Ant 风格路径]]：[[?]]（单个字符）、[[*]]（0 或多个字符）和 [[**]]（0 或多层路径）

- 支持 [[路径中的占位符]]：`{xxx}`占位符、`@PathVariable`赋值形参