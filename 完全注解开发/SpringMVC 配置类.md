---
up:
  - "[[Spring MVC 課程描述]]"
---
SpringMVC 配置类清单

- 1、扫描组件
- 2、视图解析器
- 3、view-controller
- 4、default-servlet-handler
- 5、MVC注解驱动
- 6、文件上传解析器
- 7、拦截器
- 8、异常处理解析器

```java
// 标识为配置类
@Configuration
// ========== 1、扫描组件 ==========
@ComponentScan("com.vectorx.springmvc.controller")
// ========== 5、MVC注解驱动 ==========
@EnableWebMvc
public class WebConfig {}
```