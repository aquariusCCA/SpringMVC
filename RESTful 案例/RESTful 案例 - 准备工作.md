---
up:
  - "[[Spring MVC 課程描述]]"
---
# Step1、创建工程

`File`-`New`-`Project`，默认`Next`

![[1755432634079_kb1zOsNQkN.png]]

填写项目工程基本信息，点击`FINISH`

![[1755432634079_uujx6UEyt5.png]]

---

# Step2、完善 POM

修改打包方式为`war`，并引入相关依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.vectorx</groupId>
    <artifactId>SpringMVC_RESTful</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.16</version>
        </dependency>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.11</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.15.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```

---

# Step3、web.xml

`File`-`Project Structure`-`Modules`，在`Deployment Descriptors`中点击`+`号添加`Deployment Descriptor Location`，默认路径中不带`src\main\webapp\`，需要手动添加

![[1755432634079_Qb4XCy4SWc.png]]

在`web.xml`中添加两个过滤器和一个前端控制器：

- 编码过滤器：[[处理乱码问题|CharacterEncodingFilter]]（注意顺序）
- 处理`PUT`和`DELETE`的请求过滤器：[[无 method 时匹配哪些请求方式？|HiddenHttpMethodFilter]]
- 前端控制器：[[配置 web.xml|DispatcherServlet]]

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置编码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!--配置处理 PUT 和 DELETE 的请求过滤器-->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!--配置 SpringMVC 前端控制器-->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springMVC.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

---

# Step4、SpringMVC 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--自动扫描包-->
    <context:component-scan base-package="com.vectorx.restful"></context:component-scan>

    <!--配置Thymeleaf视图解析器-->
    <bean id="ThymeleafViewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"></property>
        <property name="characterEncoding" value="UTF-8"></property>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                        <property name="prefix" value="/WEB-INF/templates/"></property>
                        <property name="suffix" value=".html"></property>
                        <property name="templateMode" value="HTML5"></property>
                        <property name="characterEncoding" value="UTF-8"></property>
                    </bean>
                </property>
            </bean>
        </property>
    </bean>

    <!--配置视图控制器-->
    <mvc:view-controller path="/" view-name="index"></mvc:view-controller>

    <!--配置MVC注解驱动-->
    <mvc:annotation-driven/>
</beans>
```

---

# Step5、创建 Controller、Dao、Bean

EmployeeController

```java
@Controller
@RequestMapping("/employeeController")
public class EmployeeController {
    @Autowired
    private EmployeeDao employeeDao;
}
```

EmployeeDao

```java
public interface EmployeeDao {
    /**
     * 增改员工信息
     *
     * @param employee
     */
    void save(Employee employee);

    /**
     * 删除员工信息
     *
     * @param id
     */
    void deleteById(Integer id);

    /**
     * 获取所有员工信息
     *
     * @return
     */
    List<Employee> getAll();

    /**
     * 根据员工id获取员工信息
     *
     * @param id
     * @return
     */
    Employee getById(Integer id);
}
```

EmployeeDaoImpl

```java
@Repository
public class EmployeeDaoImpl implements EmployeeDao {
    private static Map<Integer, Employee> employeeMap;
    private static Integer initId = 1000;

    static {
        employeeMap = new HashMap<>();
        employeeMap.put(++initId, new Employee(initId, "张三", "zhangsan@qq.com", 1));
        employeeMap.put(++initId, new Employee(initId, "李四", "lisi@qq.com", 0));
        employeeMap.put(++initId, new Employee(initId, "王五", "wangwu@qq.com", 0));
        employeeMap.put(++initId, new Employee(initId, "赵六", "zhaoliu@qq.com", 1));
    }

    @Override
    public void save(Employee employee) {
        if (employee.getId() == null) {
            employee.setId(++initId);
        }
        employeeMap.put(employee.getId(), employee);
    }

    @Override
    public void deleteById(Integer id) {
        employeeMap.remove(id);
    }

    @Override
    public List<Employee> getAll() {
        return new ArrayList<>(employeeMap.values());
    }

    @Override
    public Employee getById(Integer id) {
        return employeeMap.get(id);
    }
}
```

Employee

```java
public class Employee {
    private Integer id;
    private String lastName;
    private String email;
    private Integer gender;

    public Employee() {
    }

    public Employee(Integer id, String lastName, String email, Integer gender) {
        this.id = id;
        this.lastName = lastName;
        this.email = email;
        this.gender = gender;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getGender() {
        return gender;
    }

    public void setGender(Integer gender) {
        this.gender = gender;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "id=" + id +
                ", lastName='" + lastName + '\'' +
                ", email='" + email + '\'' +
                ", gender='" + gender + '\'' +
                '}';
    }
}
```