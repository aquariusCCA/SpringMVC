---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[SpringMVC 视图]]"
---
因为这里是使用`JSP`作为对`InternalResourceViewResolver`视图解析器的讲解，所以仅做了解即可

SpringMVC 配置文件：这里使用`InternalResourceViewResolver`代替`ThymeleafViewResolver`

```xml
<bean id="InternalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/templates/"/>
    <property name="suffix" value=".jsp"/>
</bean>
```

后台测试代码

```java
@Controller
public class JspController {
    @RequestMapping("/success")
    public String success() {
        return "success";
    }
}
```

前台测试代码

`index.jsp`

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Jsp</title>
    </head>
    <body>
        <a href="${pageContext.request.contextPath}/success">success.jsp</a>
    </body>
</html>
```

`success.jsp`

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
        <title>Success</title>
    </head>
    <body>
        <h1>Success</h1>
    </body>
</html>
```

测试结果

![[1755432634079_R4p55UOq45.gif]]