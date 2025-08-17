---
up:
  - "[[Spring MVC 課程描述]]"
---
MVC 是一种软件架构思想，将软件分为 <mark>模型、视图、控制器</mark> 三部分

- M（Model，模型层）：处理数据的 JavaBean 类
	- 实体类 Bean：存储业务数据
	- 业务处理 Bean：Service 或 Dao，处理业务逻辑和数据访问

- V（View，视图层）：展示数据的 HTML 页面，与用户进行交互

- C（Controller，控制层）：接受请求和响应的 Servlet