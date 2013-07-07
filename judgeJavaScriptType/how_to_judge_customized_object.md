如何判断自定义对象的类（型）
===========================

## instanceof运算符简介
上一篇post中，我们谈到了如何判断一个变量的类型，最常使用的就是typeof运算符，它能够准备辨别javascript内置的类型，而当遇到自定义的对象类型，就只能返回object。
为了能够准备辨别自定义对象的类型，ECMAScript引入了一种java运算符instanceof来满足这个需求。

## 引入instanceof运算符
instanceof运算符是javascript原生的用来判断实例继承关系的操作符。
深入理解instanceof运算符的用法，会对理解javascript有很大的帮助


## 使用instanceof运算符
与typeof方法不同，instanceof方法需要使用者明确地指出某种特定类型的名字。
Example：

	// initialize a string object
	var strObject = new String("hello david");
	console.log(strObject instanceof String); // should be true;
可以看到，在使用instanceof运算符的时候，
-	第一个参数：需要判断类型的变量
-	第二个参数：明确的特定类型名称，这里是String类型

再看一个更普遍的例子：
	
	function Foo(){}
	var foo = new Foo();
	console.log(foo instanceof Foo); // shoule be true

	function Aoo(){} 
	// make Foo inherit from Aoo 
	Foo.prototype = new Aoo();
	 
	console.log(foo instanceof Foo) // shoule be true
	console.log(foo instanceof Aoo) // shoule be true

上面的例子是instanceof在继承关系中的用法，当你用一个自定义类型实例化出一个对象，这个对象就是这个自定义对象的一个instance，也是这个自定义对象的父类的一个instance。这种链式的属性看起来帮助很大，其实也有隐含的问题。

请看一个看起来正常的例子：

	console.log(Object instanceof Object); // true 
	console.log(Function instanceof Function); // true 
	console.log(Function instanceof Object); //true 
	console.log(Foo instanceof Function); // true 

	console.log(Number instanceof Number); // false 
	console.log(String instanceof String); // false 	
	console.log(Foo instanceof Foo); // false
	
运行一下，你能想到这些结果么~

可能大家现在都开始有疑问了:

为什么Object和Function使用instanceof运算符作用于自己，得到true，而其他内置类instanceof自己又不等于true了, 就以上对instanceof的描述，自己本身一定是自己的一个instance。

遇到这样的疑问，一般都是从寻找这个unexpected运算符的定义着手。

## 分析instanceof运算符的定义
在ECMAScript-262 edition 3 规范中有对instanceof详细的注释，这边原文就不贴出了，分享给大家一个翻译成代码的、简易的规范：

	function instance_of(L, R) { // L 表示左表达式，R 表示右表达式
	 var O = R.prototype; // 取 R 的显示原型
	 L = L.__proto__; // 取 L 的隐式原型
	 while (true) { 
	   if (L === null) 
	     return false; 
	   if (O === L) // 这里重点：当 O 严格等于 L 时，返回 true 
	     return true; 
	   L = L.__proto__; 
	 } 
	}

 想要更好地理解上面这个代码版定义，必须熟悉掌握两个概念，即：
 - 显示原型，prototype属性
 - 隐式原型，__proto__属性
 
 这两个概念是ECMA javascript的语言的实现继承的基础。所以想要简单讲清楚并让大家理解，不是件易事，可能将来会有专门一个系列讲讲我是怎么学习这两个概念的。

接下来，通过详细讲解上文用来demo的测试用例中的三个示例：
 - Object instanceof Object
 - Function instanceod Function
 - Foo instanceof Foo

## 详解 Object instanceof Object

	// 为了方便表述，标识出左侧表达式和右侧表达式
 	ObjectL = Object, ObjectR = Object;
 	// 下面逐步推算
 	O = ObjectR. prototype = Object.prototype;
 	L = ObjectL.__proto__ = Function.prototype;
 	// 第一次判断
 	O!=L
 	// 循环查找 L 是否还有 __proto__
 	L = Function.prototype.__proto__ = Object.prototype
 	// 第二次判断
 	O === L
	// 返回 true
## 详解 Function instanceof Function

	FunctionL = Function, FunctionR = Function; 
	O = FunctionR.prototype = Function.prototype 
	L = FunctionL.__proto__ = Function.prototype 
	// 第一次判断
	O ===L 
	// 返回 true
	

## 详解 Foo instanceof Foo

	FooL = Foo, FooR = Foo; 
	O = FooR.prototype = Foo.prototype 
	L = FooL.__proto__ = Function.prototype 
	// 第一次判断
	O != L 
	// 循环再次查找 L 是否还有 __proto__ 
	L = Function.prototype.__proto__ = Object.prototype 
	// 第二次判断
	O != L 
	// 再次循环查找 L 是否还有 __proto__ 
	L = Object.prototype.__proto__ = null
	// 第三次判断
	L == null
	// 返回 false

 需要指出的是:

 - 只有Object.prototype.__proto__ === null

 - 所有内置类型或者自定义类型的__proto__ === Function.prototype

 - Function.prototype.__proto__ === Object.prototype.
 
## 结束语
以上，简单介绍了Javascript中另一种判断自定义对象类型的方法，instanceof。也通过实例讲解了instanceof运算符的内部实现，帮着我们了解了一些unexpected结果的原因。
 
- 以上测试结果均在chrome浏览器运行得出。
- [参考link][JavaScript_instanceof_operator]
[JavaScript_instanceof_operator]: http://blog.jobbole.com/41611/
