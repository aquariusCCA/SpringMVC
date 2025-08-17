---
up:
  - "[[Spring MVC 課程描述]]"
---
```xml
<!--自动扫描包-->
<context:component-scan base-package="com.vectorx.springmvc"></context:component-scan>

<!--配置Thymeleaf视图解析器-->
<bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
        <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
            <property name="templateResolver">
                <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                    <!--视图前缀-->
                    <property name="prefix" value="/WEB-INF/templates/"/>
                    <!--视图后缀-->
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                    <property name="characterEncoding" value="UTF-8"/>
                </bean>
            </property>
        </bean>
    </property>
</bean>
```