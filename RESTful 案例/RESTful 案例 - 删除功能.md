---
up:
  - "[[Spring MVC 課程描述]]"
---
在`webapp`下新建`static/css`和`static/js`，用来放置`css`文件和`js`文件

- 引入`static/js/vue.js`
- `static/css/employeelist.css`作为外部样式文件
- `static/js/employeelist.js`作为外部`js`文件

employeelist.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>员工信息</title>
        <link th:href="@{/static/css/employeelist.css}" rel="stylesheet" type="text/css"/>
    </head>
    <body>
        <table id="employeeTable">
            <tr>
                <th colspan="5">员工信息</th>
            </tr>
            <tr>
                <th>ID</th>
                <th>姓名</th>
                <th>邮箱</th>
                <th>性别</th>
                <th>操作</th>
            </tr>
            <tr th:each="employee : ${employeeList}">
                <td th:text="${employee.id}"></td>
                <td th:text="${employee.lastName}"></td>
                <td th:text="${employee.email}"></td>
                <td th:text="${employee.gender == 1 ? '男' : '女'}"></td>
                <td>
                    <a href="">修改</a>
                    <!--<a th:href="@{/employeeController/employee/}+${employee.id}">删除</a>-->
                    <a @click="deleteEmployee" th:href="@{'/employeeController/employee/'+${employee.id}}">删除</a>
                </td>
            </tr>
        </table>
        <form method="post" id="deleteForm">
            <input type="hidden" name="_method" value="delete"/>
        </form>
        <script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
        <script type="text/javascript" th:src="@{/static/js/employeelist.js}"></script>
    </body>
</html>
```

employeelist.css

```css
table {
    width: 50%;
    border: 1px black solid;
    border-collapse: collapse;
    /* 垂直水平居中 */
    vertical-align: middle;
    text-align: center;
}

/* 间隔变色 */
tbody tr:nth-child(odd) {
    background-color: rgb(211, 216, 188);
}

th, td {
    border: 1px black solid;
}
```

employeelist.js

```js
var vue = new Vue({
    el: "#employeeTable",
    methods: {
        deleteEmployee: function (event) {
            if (confirm('确认删除吗？')) {
                var deleteForm = document.getElementById("deleteForm");
                deleteForm.action = event.target.href;
                deleteForm.submit();
            }
            event.preventDefault();
        }
    }
});
```

SpringMVC 配置文件

```xml
<!--开放静态资源访问-->
<mvc:default-servlet-handler/>
```

效果

![[1755503612826_9UTv775kBL.gif]]