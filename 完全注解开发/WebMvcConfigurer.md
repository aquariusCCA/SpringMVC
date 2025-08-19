---
up:
  - "[[Spring MVC 課程描述]]"
---
```java
@Configuration
@ComponentScan("com.vectorx.springmvc.controller")
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
    // 略...

    // ========== 3、view-controller ==========
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
    }
    // ========== 4、default-servlet-handler ==========
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    // ========== 7、拦截器 ==========
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new TestInterceptor()).addPathPatterns("/**");
    }

    // ========== 8、异常处理解析器 ==========
    @Override
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();
        Properties properties = new Properties();
        properties.setProperty("java.lang.ArithmeticException", "error");
        exceptionResolver.setExceptionMappings(properties);
        exceptionResolver.setExceptionAttribute("ex");
        resolvers.add(exceptionResolver);
    }
}
```