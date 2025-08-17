---
up:
  - "[[Spring MVC 課程描述]]"
---
`@RequestParam` 是 ==将请求参数和控制器方法的形参创建映射关系==

一共有三个属性：

- `value`：指定为形参赋值的请求参数的参数名
- [[@RequestParam 的 required | required]]`：设置是否必须传输此请求参数，默认值为`true`
    - 若设置为`true`，则当前请求必须传输`value`所指定的请求参数，若没有传输该请求参数，且没有设置`defaultValue`属性，则页面报错`400：Required String parameter'xxx'is not present`；
    - 若设置为`false`，则当前请求不是必须传输`value`所指定的请求参数，若没有传输，则注解所标识的形参的值为`null`
- [[@RequestParam 的 defaultValue | defaultValue]]：不管`required`属性值为`true`或`false`，当`value`所指定的请求参数没有传输或传输的值为空值时，则使用默认值为形参赋值

实际开发中，请求参数与控制器方法形参未必一致，一旦出现这种情况，还能否接收到请求参数了呢？

这里简单地将前台`name="username"`改为`name="user_name"`进行测试，看下后台日志信息，果然没有接收到 user_name 这个请求参数

```java
username=null, password=aaaaaaaa, hobby=[Spring, SpringMVC, SpringBoot]
```

**扩展思考**：这里也侧面证明一件事，SpringMVC 中对请求参数的赋值是根据是否同名来决定的，而不会根据参数在方法上的第几个位置决定，也就是说 SpringMVC 没有考虑将 _请求参数个数、类型与顺序_ 与 _控制器方法形参个数、类型与顺序_ 进行绑定。

如果我们来设计 SpringMVC，应该考虑这种方案么？

个人觉得，这种方案虽然可以实现与 Java 重载方法的一一绑定关系，但实际操作起来有一定难度：

- 比如数字类型可以当作 String 处理，也可以当作 Integer 处理，不好区分
- 退一步来讲，如果考虑重载方法，SpringMVC 底层势必要对类中所有重载方法进行循环，判断是否满足个数、类型和顺序的要求，性能上一定有所影响

而限制请求路径和请求方式不能完全相同的话，就没有这种苦恼了。即使是重载方法，通过不同请求路径或请求方法来界定到底访问哪个方法就可以了

SpringMVC 借助注解的方式，将请求参数与控制器方法形参关系绑定的决定权，交到开发者的手中。这种开发思维启发我们，如果有些功能不能很好地在底层进行实现，甚至可能会留下很多隐患时，还不如交给实际使用者，由他们去决定，否则很容易被使用者诟病（没有，我没有暗示某语言啊(●'◡'●)）

此时使用`@RequestParam`注解就可以实现请求参数与控制器方法形参的绑定

后台测试代码

```java
@RequestMapping("/testParam3")
public String testParam3(@RequestParam("user_name") String username, String password, String[] hobby) {
    System.out.println("username=" + username + ", password=" + password + ", hobby=" + Arrays.toString(hobby));
    return "success";
}
```

后台日志信息

```shell
username=ss, password=aaaaa, hobby=[Spring, SpringMVC, SpringBoot]
```

关于`@RequestParam`怎么使用，可以看下源码

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface RequestParam {
    @AliasFor("name")
    String value() default "";
    
    @AliasFor("value")
    String name() default "";
    
    boolean required() default true;
    
    String defaultValue() default ValueConstants.DEFAULT_NONE;
}
```

- `name`和`value`：绑定的请求参数名，互为别名，用哪个都一样
- `required`属性：默认为`true`，表示必须要有此参数，不传则报错；不确定是否传参又不想报错，赋值为`false`即可
- `defaultValue`属性：不管`required`是`true`还是`false`，只要请求参数值为空（`""`或`null`），就为形参附上此值

