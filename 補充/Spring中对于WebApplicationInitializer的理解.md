---
up:
  - "[[AbstractAnnotationConfigDispatcherServletlnitializer]]"
url: https://blog.csdn.net/u011047968/article/details/106180483
---
# 1、前言

在 [《SpringMVC学习（五）——零配置实现SpringMVC》](https://blog.csdn.net/u011047968/article/details/106172250) 这篇文章中我们没有使用 Spring 的配置实现了一个正常的SpringMVC的功能，里面核心的一个点就是使用了 WebApplicationInitializer，那这篇文章就详细说明一下这个接口的作用。

---

# 2、WebApplicationInitializer 的定义

从起初的 Spring 配置文件，到后来的 Spring 支持注解到后来的 SpringBoot，Spring 框架在一步步的使用注解的方式来去除 Spring 的配置的发展过程。WebApplicationInitializer 就是取代 web.xml 配置的一个接口。

```java
public interface WebApplicationInitializer {
    void onStartup(ServletContext var1) throws ServletException;
}
```

通过覆盖接口提供的 onStartup 方法我们可以往 Servlet 容器里面添加我们需要的 servlet、listener等，并且在 Servlet 容器启动的过程中就会加载这个接口的实现类，从而起到和 web.xml 相同的中作用，从而可以替代以前在 web.xml 中所做的配置。

---

# 3、实现原理

我们首先可以从 Spring 源码中找到 SpringServletContainerInitializer 实现类。

```java
@HandlesTypes({WebApplicationInitializer.class})
public class SpringServletContainerInitializer implements ServletContainerInitializer {
    public SpringServletContainerInitializer() {
    }

    public void onStartup(Set<Class<?>> webAppInitializerClasses, ServletContext servletContext) throws ServletException {
        LinkedList initializers = new LinkedList();
        Iterator var4;
        if(webAppInitializerClasses != null) {
            var4 = webAppInitializerClasses.iterator();

            while(var4.hasNext()) {
                Class initializer = (Class)var4.next();
                if(!initializer.isInterface() && !Modifier.isAbstract(initializer.getModifiers()) && WebApplicationInitializer.class.isAssignableFrom(initializer)) {
                    try {
                        initializers.add((WebApplicationInitializer)initializer.newInstance());
                    } catch (Throwable var7) {
                        throw new ServletException("Failed to instantiate WebApplicationInitializer class", var7);
                    }
                }
            }
        }

        if(initializers.isEmpty()) {
            servletContext.log("No Spring WebApplicationInitializer types detected on classpath");
        } else {
            servletContext.log(initializers.size() + " Spring WebApplicationInitializers detected on classpath");
            AnnotationAwareOrderComparator.sort(initializers);
            var4 = initializers.iterator();

            while(var4.hasNext()) {
                WebApplicationInitializer initializer1 = (WebApplicationInitializer)var4.next();
                initializer1.onStartup(servletContext);
            }

        }
    }
}
```

这个类上面有`@HandlesTypes({WebApplicationInitializer.class})`这个注解，这个注解的作用是将其 value 中配置的一些类放入到`ServletContainerInitializer`。

```java
initializers.add((WebApplicationInitializer)initializer.newInstance());
```

最后通过循环去执行`WebApplicationInitializer`中的`onStartup`方法来实现里面的具体的具体的逻辑。

```java
while(var4.hasNext()) {
	WebApplicationInitializer initializer1 = (WebApplicationInitializer)var4.next();
	initializer1.onStartup(servletContext);
}
```

## 那问题来了，Tomcat 容器怎么知道先执行这个 SpringServletContainerInitializer 类 ？

这里涉及一个知识点 SPI 机制，SPI 全称为 Service Provider Interface，是一种服务发现机制。SPI机制是将接口实现类的全限定名配置在文件中，并由服务加载器读取配置文件，加载实现类。这样可以在运行时，动态为接口替换实现类。正因此特性，我们可以很容易的通过 SPI机制为我们的程序提供拓展功能。

Tomcat 启动过程中查找所有的 ServletContainerInitializer 实现类然后添加到 StandardContext 的initializers 集合中，然后执行里面的 onStartup 方法。

Spring 中 SPI 就是通过 SpringServletContainerInitializer 类来实现的

![[7b54f3d0e70938afc51ffa7ffd9750ba.png]]

---

# 4、利用 SPI 我们能做什么？

可以加一些自己的启动配置信息，把自己的 Servlet 打成 jar 包放到 Tomcat 服务器或者其他工程中执行。我们可以实现一个自己的 SPI 接口。

## 4.1、定义一个`MyWebAppInitializer`

```java
public interface MyWebAppInitializer {
    void loadOnStart(ServletContext var1) throws ServletException;
}
```

## 4.2、定义一个 MySpringServletContainerInitializer

1. 实现 ServletContainerInitializer 接口

2. 修改 @HandlesTypes 为我们自己定义的 MyWebAppInitializer.class

3. 实例化 MyWebAppInitializer 的实现类，并且调用接口的 loadOnStart 方法

```java
@HandlesTypes(MyWebAppInitializer.class)
public class MySpringServletContainerInitializer implements ServletContainerInitializer{
    @Override
    public void onStartup(Set<Class<?>> set, ServletContext servletcontext)
            throws ServletException {
        if (set != null) {
            for (Class<?> waiClass : set) {
                if (!waiClass.isInterface() && !Modifier.isAbstract(waiClass.getModifiers()) &&
                        MyWebAppInitializer.class.isAssignableFrom(waiClass)) {
                    try {
                        //创建MyWebAppInitializer实现类的对象，并调用loadOnStart方法
                        ((MyWebAppInitializer) waiClass.newInstance()).loadOnStart(servletcontext);
                    } catch (Throwable ex) {
                        throw new ServletException("Failed to instantiate WebApplicationInitializer class", ex);
                    }
                }
            }
        }
    }
}
```

## 4.3、写一个`MyWebAppInitializer`的实现类

```java
public class MyWebApplicationInitializerTest implements MyWebAppInitializer{
    @Override
    public void loadOnStart(ServletContext servletContext){
        System.out.println("启动执行MyWebApplicationInitializerTest的loadOnStart方法");
        //注册一个为名字call的servlet
        ServletRegistration.Dynamic servletReg = servletContext.addServlet("call", CallServlet.class);
        servletReg.setLoadOnStartup(1);
        servletReg.addMapping("/call");
    }
}
```

其中里面的`CallServlet.java`如下

```java
public class CallServlet extends HttpServlet {
    private static final long serialVersionUID = 3684613967452881093L;

    @Override
    public void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String name = req.getParameter("name");
        resp.getWriter().write(name + ", if you like me, please call me!");
    }
}
```

## 4.4、添加配置文件 javax.servlet.ServletContainerInitializerr

添加位置为：src -> main -> resources -> META-INF -> services -> javax.servlet.ServletContainerInitializerr

内容：com.leo.spi.MySpringServletContainerInitializer

![[5400895be6dbcd68100dd28ec452a1af.png]]

## 4.5、启动项目测试

启动日志：

```java
启动执行MyWebApplicationInitializerTest的loadOnStart方法
```

浏览器测试：[http://localhost:8080/springmvc/call?name=leo825](http://localhost:8080/springmvc/call?name=leo825)

![[54d0ecb96c182fbab233b14086b99751.png]]