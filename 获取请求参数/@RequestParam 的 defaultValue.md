---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[@RequestParam]]"
---
后台测试代码

```java
@RequestMapping("/testParam3")
public String testParam3(
    @RequestParam(value = "user_name", required = false, defaultValue = "heh") String username,
    String password, String[] hobby) {
    System.out.println("username=" + username + ", password=" + password + ", hobby=" + Arrays.toString(hobby));
    return "success";
}
```

请求路径：传空值和不传值两种情况

```ini
/testParam3?user_name=&password=asdf&hobby=Spring&hobby=SpringMVC
/testParam3?password=asdf&hobby=Spring&hobby=SpringMVC
```

后台日志信息

```shell
username=heh, password=asdf, hobby=[Spring, SpringMVC]
```

可以发现，不管是为 username 传空值还是不传值，最终都会被赋上默认值

这里将`required`修改为`true`，即默认值的情况，发现也是可以请求成功的

> **注意**：`required`一节测试中，在`required`的默认值情况下，没有为请求参数赋值传值或传空值，会产生`400`的错误。
> 
> 而只要为请求参数设置默认值，即使用`@RequestParam`注解的`defaultValue`属性赋上值，就不会有`400`错误了。
> 
> 换句话说，只要设置了`defaultValue`属性值，`required`属性就失效形同虚设了