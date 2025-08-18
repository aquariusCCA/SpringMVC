---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[HttpMessageConverter]]"
  - "[[@ResponseBody]]"
---
`@RestController`注解是 SpringMVC 提供的一个复合注解，标识在控制器的类上，就相当于为类添加了`@Controller`注解，并且为其中的每个方法添加了`@ResponseBody`注解

这里简单修改下后台代码，将`@Controller`注解替换为`@RestController`注解，并去除控制器方法上的`@ResponseBody`注解

```java
@RestController
@RequestMapping("/httpController")
public class HttpController {
    @PostMapping("/testAxios")
    public String testAxios(User user) {
        return user.getUsername() + "," + user.getPassword();
    }
}
```

测试效果

![[1755508001254_jYkCKDScqY.gif]]