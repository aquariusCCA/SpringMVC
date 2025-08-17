---
up:
  - "[[Spring MVC 課程描述]]"
---
分别在上述对应的控制器方法中，添加打印 Model、ModelMap 和 Map 三个对象及其对应类名的逻辑

```java
System.out.println(model + "======" + model.getClass().getName());
System.out.println(map + "======" + map.getClass().getName());
System.out.println(modelMap + "======" + modelMap.getClass().getName());
```

通过分别点击前台超链接，并查看后台日志信息

```shell
{testRequestScope=hello, Model!}======org.springframework.validation.support.BindingAwareModelMap
{testRequestScope=hello, Map!}======org.springframework.validation.support.BindingAwareModelMap
{testRequestScope=hello, ModelMap!}======org.springframework.validation.support.BindingAwareModelMap
```

可以发现

- Model、ModelMap 和 Map 三个对象输入格式是一致的，都为键值对形式
- 通过反射方法获取到的类都是同一个，即`BindingAwareModelMap`

查看`BindingAwareModelMap`的继承关系

![[1755431156554_0rQTZTHPm3.png]]

阅读源码，梳理出`Model`、`Map`、`ModelMap`三者的核心继承关系

```java
public class BindingAwareModelMap extends ExtendedModelMap {}
public class ExtendedModelMap extends ModelMap implements Model {}
public class ModelMap extends LinkedHashMap<String, Object> {}
public interface Model {}
```

可以发现

- `BindingAwareModelMap`继承`ModelMap`并实现`Model`接口
- `ModelMap`继承`LinkedHashMap`，而毫无疑问`LinkedHashMap`实现了`Map`接口

`Model`、`Map`和`ModelMap`三者的关系到此就一目了然了，其 UML 类图如下：

![[1755431156554_uNBs7rtqgn.png]]

> **结论**：`Model`、`Map`、`ModelMap`类型的形参本质上都是`BindingAwareModelMap`