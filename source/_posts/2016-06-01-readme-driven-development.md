---
layout: post
title: 函数定义、重载，函数声明与表达式
time: 2016年07月1日 星期四
location: 北京
pulished: true
excerpt_separator: "```"
---


- javascript的函数也是对象,实际上他是Function类型的实例 有三种写法
- 函数声明,**会被解析器率先读取,并在执行任何代码之前可用**
- 因为在代码执行之前,解析器就通过一个函数声明提升(function declaration hoisting)的过程,读取函数声明到执行环境中


<!--more-->

```js
		console.log(sum(2,4));//虽然写在函数声明之前但是依然可以被执行
		function sum(sum1,sum2) {
		    return sum1+sum2;
		
		}
		console.log(sum(1,2));
```



- 函数表达式 在解析器执行到该种写法的函数时,函数才会真正被解析执行
- 也就是说在执行到函数所在的语句之前,变量aaa不会保存有函数的引用

```js
	var aaa = function (sum1,sum2) {
		return sum1+sum2;
	}
	console.log(aaa(1,2));
```


- 不推荐写法,这种写法会导致解析两次代码,第一次是常规解析ECMAScript,第二次是解析解析传入构造函数中的字符串
- 不过这种写法对于理解函数是对象,函数名是指针的概念是很直观的

```js
		var bbb = new Function("sum1","sum2","return sum1+sum2");
		console.log(bbb(2,3));
```

- 使用不带括号的函数名是访问函数指针,不是调用函数

```js
console.log(bbb);   //[Function]
function sums(a,b) {
    return a+b;
}
var anothersum = sums;  // 访问指针
console.log(anothersum(1,2));
var sums = null;
console.log(anothersum(2,3));
```

- 函数没有重载

```js
		function iszl(a) {
		    return a+100;
		}
		function iszl(a) {
		    return a+200;
		}
		console.log(iszl(100)); //300
```
- 创建第二个函数时,实际上覆盖了引用第一个函数的变量(指针的概念)

```js
		var addnum = function (a) {
		    return a+100;
		}
		addnum = function (a) {
		    return a+200;
		}
		console.log(addnum(100))
```


- 