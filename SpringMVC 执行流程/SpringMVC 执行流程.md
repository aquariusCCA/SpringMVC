---
up:
  - "[[Spring MVC 課程描述]]"
---
1）用户向服务器发送请求，请求被 SpringMVC 前端控制器`DispatcherServlet`捕获

2）`DispatcherServlet`对请求 URL 进行解析，得到请求资源标识符 URI ，判断请求 URI 对应的映射是否存在：

若不存在，再判断是否配置了`mvc:default-servlet-handler`

- i.如果没配置，则控制台报映射查找不到，客户端展示 404 错误

```shell
DEBUG org.springframework.web.servlet.Dispatcherservlet - GET "/springMVC/testHaha", parameters={}
WARN org.springframework.web.servlet.PageNotFound - No mapping for GET /springMVC/testHaha 
DEBUG org.springframework.web.servlet.Dispatcherservlet - Completed 404 NOT_FOUND
```

- ii.如果有配置，则访问目标资源（一般为静态资源，如：JS，CSS，HTML），找不到客户端也会展示404错误

```shell
  DEBUG org.springframework.web.servlet.Dispatcherservlet - GET "/springMVC/testHaha", parameters={}
  handler.SimpleUrlHandlerMapping Mapped to org.springframework.web.servlet.resource.DefaultServletHttpRequestHandlerDispatcherservlet - Completed 404 NOT_FOUND
```

若存在，则执行以下流程

3）根据该 URI，调用`HandlerMapping`获得该`Handler`配置的所有相关的对象（包括`Handler`对象以及`Handler`对象对应的拦截器），最后以`HandlerExecutionChain`执行链对象的形式返回

4）`DispatcherServlet`根据获得的`Handler`，选择一个合适的`HandlerAdapter`

5）如果成功获得`HandlerAdapter`，此时将开始执行拦截器的`preHandler()`方法【正向】

6）提取 Request 中的模型数据，填充`Handler`入参，开始执行`Handler`（Controller）方法，处理请求。在填充`Handler`的入参过程中，根据你的配置，Spring 将帮你做一些额外的工作：

- a）`HttpMessageConveter`：将请求消息（如 json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息
- b）数据转换：对请求消息进行数据转换。如 String 转换成 Integer、Double 等
- c）数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等
- d）数据验证：验证数据的有效性（长度、格式等），验证结果存储到 BindingResult 或 Error 中

7）`Handler`执行完成后，向`DispatcherServlet`返回一个`ModelAndView`对象

8）此时将开始执行拦截器的`postHandle()`方法【逆向】

9）根据返回的`ModelAndView`（此时会判断是否存在异常，如果存在异常，则执行`HandlerExceptionResolver`进行异常处理）选择一个适合的`ViewResolver`进行视图解析，根据`Model`和`View`，来渲染视图

10）渲染视图完毕执行拦截器的`afterCompletion()`方法【逆向】

11）将渲染结果返回给客户端