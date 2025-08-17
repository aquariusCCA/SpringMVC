---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[@RequestParam]]"
---
> `required`默认为`true`，即要求该请求参数不能为空。因为是默认值，所以添加`required="true"`与不写`required`属性是一样的

这里先测试下默认情况下不传对应请求参数时系统的反应如何，只需要将`user_name`一行注释即可，或直接在浏览器地址栏删除该请求参数也一样

测试结果

![[1755428033319_kx46NWOYq9.png]]

报错信息：`400`错误的请求，必须的请求参数'user_name'...不存在

```shell
HTTP Status 400 - Required request parameter 'user_name' for method parameter type String is not present
```

经测试，不论是为 username 传空值还是不传值，都是`400`错误

```shell
/testParam3?user_name=&password=11&hobby=Spring&hobby=SpringMVC
/testParam3?password=11&hobby=Spring&hobby=SpringMVC
```

如果将`required`设置为`false`，还会报错吗？

后台测试代码：只需要对`@RequestParam("user_name")`稍作改动，修改为`@RequestParam(value = "user_name", required = false)`即可

```java
@RequestMapping("/testParam3")
public String testParam3(@RequestParam(value = "user_name", required = false) String username, String password, String[] hobby) {
    System.out.println("username=" + username + ", password=" + password + ", hobby=" + Arrays.toString(hobby));
    return "success";
}
```

测试结果：可以发现，这次并没有报`400`错误

![[1755428033319_vVYATb4UyW.png]]

后台日志信息

```java
username=null, password=1111, hobby=[Spring, SpringMVC]
```

这是不传 user_name 的情况，如果是传空值呢？

测试结果：同样访问成功，没有报`400`错误

![[1755428033319_fczXfz3dyp.png]]

后台日志信息

```java
username=, password=111, hobby=[Spring, SpringMVC]
```

> **Q**：不是说默认是`true`吗？为什么在没有使用`@RequestParam`注解时，也能正常访问呢？
> 
> **A**：这个默认值本身就是在使用`@RequestParam`注解时生效的，如果都没有使用到`@RequestParam`，就没有相应限制了