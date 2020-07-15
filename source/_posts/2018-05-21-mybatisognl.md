---
layout: post_layout
title: 「MyBatis」MyBatis中的OGNL用法
time: 2018年05月21日 星期一
location: 北京
pulished: true
excerpt_separator: "```"
---

在MyBatis的动态SQL和${}形式的参数中都用到了OGNL表达式，所以这里介绍一些简单用法。

<!--more-->

a、b均是变量
1. a `or` b
2. a `and` b
3. a `==` b 或者 a `eq` b
4. a `!=` b 或者 a `neq` b
5. 小于：a `lt` b
6. 小于等于：a `lte` b
7. 大于：a `gt` b
8. 大于等于：a `gte` b
9. 运算符：a`+`b a`*`b a`/`b a`-`b a`%`b
10. 非/取反：`!`a 或者 `not` a
11. 调用方法：a`.method(`args`)`
12. 对象取属性值：a.propertie
13. 按索引取值：a`[`i`]`
14. 调用类的静态方法：`@`class`@`method(args)
15. 调用类的静态字段：`@`class`@`field

表达式1-4是最常用的四种情况。另外有些时候需要判断一个集合是否为空时，可能会用如下判断。
```xml
<if test="list != null and list.size() > 0">
<!--其他-->
</if>
```

表达式12-13两种情况也比较常见，而且可以多层嵌套使用。假设User类型的属性user中又一个Address类型的属性名
为addr，在Address中还有一个属性zipcode，可以通过user.addr.zipcode直接使用zipcode的值。
假设Map类型的属性为map，我们可以通过map`[`'userName'`]`或者map.userName来获取map中key为userName的值。
这里一定要注意，不管userName的值是不是null，必须保证userName这个key存在，否则就会报错。

表达式14通常用于简化一些校验，或者进行更特殊的校验，例如在if中常见的判断可以写成如下所示。
```xml
<if test="@com.demo.util.StringUtil@isNotEmpty(username)">
    and user_name like concat('%',#{username},'%')
</if>
```

其中StringUtil中的静态方法isNotEmpty()用来判断字符串是否为空。

这种写法的一个特殊用法，加入在测试的时候想知道映射XML咋方法执行的参数，可以在上面的StringUtil方法中添加如下方法：
```java
public static void print(Object patameter){
    System.out.println(parameter);
}
```
然后在映射文件的方法标签内添加如下方法。
```xml
<bind name="print" value="@com.demo.util.StringUtil@print(_parameter)">
```
通过上面的配置可以启用一些特殊的功能。



