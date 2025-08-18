---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[RESTful 简介]]"
---
`RESTful`的实现，具体说就是：HTTP 协议里面，四个表示操作方式的动词`GET`、`POST`、`PUT`、`DELETE`

它们分别对应四种基本操作：

- `GET`用来获取资源
- `POST`用来新建资源
- `PUT`用来更新资源
- `DELETE`用来删除资源

`REST`风格 `URL` 地址不使用 _问号键值对方式_ 携带请求参数，而是：

==提倡使用统一的风格设计，从前到后各个单词使用斜杠分开，将要发送给服务器的数据作为`URL`地址的一部分，以保证整体风格的一致性==

> 以往，我们访问资源的方式五花八问。例如，
> 
> - 获取用户信息通过`getUserById`/`selectUserById`/`findUserById`/等
> - 删除用户信息通过`deleteUserById`/`removeUserById`等
> - 更新用户信息通过`updateUser`/`modifyUser`/`saveUser`等
> - 新增用户信息通过`addUser`/`createUser`/`insertUser`等
> 
> 上述操作的资源都是用户信息。按照`RESTful`思想，既然操作的资源一样，那么请求路径就应该一样

用一张表格来对比传统方式和`REST`风格对资源操作的区别

|操作|传统方式|REST 风格|
|:--|:--|:--|
|查询|`getUserById?id=1`|`user/1`-->`get`请求|
|保存|`saveUser`|`user`-->`post`请求|
|删除|`deleteUserById?id=1`|`user/1`-->`delete`请求|
|更新|`updateUser`|`user`-->`put`请求|