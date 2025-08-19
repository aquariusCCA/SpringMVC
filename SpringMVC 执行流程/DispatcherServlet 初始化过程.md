---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[SpringMVC 执行流程]]"
---
> `DispatcherServlet`本质上是一个 Servlet，所以天然的遵循 Servlet 的生命周期，宏观上是 Servlet 生命周期来进行调度

![[1755592581924_cYEqTi3o8O.png]]

---

# 初始化 WebApplicationContext

所在类：`org.springframework.web.servlet.FrameworkServlet`

```java
protected WebApplicationContext initWebApplicationContext() {
    WebApplicationContext rootContext =
        WebApplicationContextUtils.getWebApplicationContext(getServletContext());
    WebApplicationContext wac = null;
    if (this.webApplicationContext != null) {
        wac = this.webApplicationContext;
        if (wac instanceof ConfigurableWebApplicationContext) {
            ConfigurableWebApplicationContext cwac = (ConfigurableWebApplicationContext) wac;
            if (!cwac.isActive()) {
                if (cwac.getParent() == null) {
                    cwac.setParent(rootContext);
                }
                configureAndRefreshWebApplicationContext(cwac);
            }
        }
    }
    if (wac == null) {
        wac = findWebApplicationContext();
    }
    if (wac == null) {
        // 创建ApplicationContext
        wac = createWebApplicationContext(rootContext);
    }
    if (!this.refreshEventReceived) {
        synchronized (this.onRefreshMonitor) {
            // 刷新ApplicationContext
            onRefresh(wac);
        }
    }
    if (this.publishContext) {
        String attrName = getServletContextAttributeName();
        // 将IOC容器在应用域共享
        getServletContext().setAttribute(attrName, wac);
    }
    return wac;
}
```

---

# 创建 WebApplicationContext

所在类：`org.springframework.web.servlet.FrameworkServlet`

```java
protected WebApplicationContext createWebApplicationContext(@Nullable ApplicationContext parent) {
    Class<?> contextClass = getContextClass();
    if (!ConfigurableWebApplicationContext.class.isAssignableFrom(contextClass)) {
        throw new ApplicationContextException(
            "Fatal initialization error in servlet with name '" + getServletName() +
            "': custom WebApplicationContext class [" + contextClass.getName() +
            "] is not of type ConfigurableWebApplicationContext");
    }
    // 反射创建IOC容器对象
    ConfigurableWebApplicationContext wac =
        (ConfigurableWebApplicationContext) BeanUtils.instantiateClass(contextClass);
    wac.setEnvironment(getEnvironment());
    // 设置父容器：将Spring上下文对象作为SpringMVC上下文对象的父容器
    wac.setParent(parent);
    String configLocation = getContextConfigLocation();
    if (configLocation != null) {
        wac.setConfigLocation(configLocation);
    }
    configureAndRefreshWebApplicationContext(wac);
    return wac;
}
```

---

# DispatcherServlet 初始化策略

`FrameworkServlet`创建 `WebApplicationContext` 后，调用`onRefresh(wac)`刷新容器

此方法在`DispatcherServlet`中进行了重写，调用了`initStrategies(context)`方法，初始化策略，即初始化`DispatcherServlet`的各个组件

所在类：`org.springframework.web.servlet.DispatcherServlet`

```java
protected void initStrategies(ApplicationContext context) {
    // 初始化文件上传解析器
    initMultipartResolver(context);
    initLocaleResolver(context);
    initThemeResolver(context);
    // 初始化处理器映射
    initHandlerMappings(context);
    // 初始化处理器适配器
    initHandlerAdapters(context);
    // 初始化处理器异常处理器
    initHandlerExceptionResolvers(context);
    initRequestToViewNameTranslator(context);
    // 初始化视图解析器
    initViewResolvers(context);
    initFlashMapManager(context);
}
```