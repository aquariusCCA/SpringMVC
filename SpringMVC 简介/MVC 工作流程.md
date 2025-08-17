---
up:
  - "[[Spring MVC 課程描述]]"
---
1. 用户通过视图层发送请求到服务器，在服务器中请求被 Controller 接收

2. Controller 调用相应的 Model 层处理请求

3. Model 层处理完毕将结果返回到 Controller

4. Controller 再根据请求处理的结果找到相应的 View 视图

5. View 视图渲染数据后最终响应给浏览器