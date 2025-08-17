---
up:
  - "[[Spring MVC 課程描述]]"
  - "[[@RequestMapping 的 method 属性]]"
---
`@RequestMapping`注解的`params`属性通过请求的请求参数匹配请求映射

它是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

- [[param]]：要求请求映射所匹配的请求必须携带`param`请求参数
- [[!param]]：要求请求映射所匹配的请求必须不能携带`param`请求参数
- [[param=value]]：要求请求映射所匹配的请求必须携带`param`请求参数且`param=value`
- [[param!=value]]：要求请求映射所匹配的请求必须携带`param`请求参数但是`param!=value`

若当前请求满足`@RequestMapping`注解的`value`和`method`属性，但是不满足`params`属性，此时页面显示`400`错误，即请求不匹配