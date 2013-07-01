Javascirpt中如何做对象的类型判断
================================

一般来说，有两种方式：
- 使用typeof
- 使用constructor

## typeof的使用
typeof是做类型判断时最常用的方法。

优点：简单，好记。

缺点：不能很好地判断object、null、array、regexp和自定义对象。

Example:

	var str ='str';
	var arr = ['1','2'];
	var num = 1;
	var bool = true;
	var obj = {name: 'test'};
	var nullObj = null;
	var undefinedObj = undefined;
	var reg =/reg/;
	function fn(){
    alert('this is a function');
	}

	function User(name){
	    this.name = name;
	}

	var user = new User('user name');

	console.log(typeof str);
	console.log(typeof arr);
	console.log(typeof num);
	console.log(typeof bool);
	console.log(typeof nullObj);
	console.log(typeof undefinedObj);
	console.log(typeof reg);
	console.log(typeof fn);
	console.log(typeof user);

Run Result:

![run result of typeof][typeof_run_result]

可以发现，对于Array null regexp 和自定义对象进行typeof运算，都得到了相同的结果：object。

## constructor 使用
对象构造器constructor是一种不常使用的方法。

优点：支持大部分对象类型的判断，特别是对自定义对象的判断。

缺点：不能在null和undefined上使用。

Example: 测试代码与之前一致。

	console.log(str.constructor);
	console.log(arr.constructor);
	console.log(num.constructor);
	console.log(bool.constructor);
	console.log(reg.constructor);
	console.log(fn.constructor);
	console.log(user.constructor);

	console.log(nullObj.constructor);
	console.log(undefinedObj.constructor);

Run Result:

![run result of constructor][constructor_run_result]

可以发现，当在null对象或者undefined对象上使用constructor运算，浏览器会报错。

## 有没有更完备的方法来判断对象类型
综上两种方法，都有不完备的地方，而且我们还特别需要那些地方，所以能不能找到一种更好的方法呢？

答案是， let us try...

## 接近完备的Object.prototype.toString.call()
Object.prototype.toString.call()是jquery中使用的方式。它跟对象原生的toString()有什么不同，这里就不再赘述，有兴趣的同学，可以自行google。

优点：支持绝大多数的类型判断。

缺点：不支持自定义对象的判断。

Example：测试代码一致。

	var toString=Object.prototype.toString;

	console.log(toString.call(str));
	console.log(toString.call(arr));
	console.log(toString.call(num));
	console.log(toString.call(bool));
	console.log(toString.call(obj));
	console.log(toString.call(reg));
	console.log(toString.call(fn));
	console.log(toString.call(user));
	console.log(toString.call(nullObj));
	console.log(toString.call(undefinedObj));

Run Result:

![run result of Object.prototype.toString.call()][call_run_result]

[typeof_run_result]: img/result1.png
[constructor_run_result]: img/result2.png
[call_run_result]: img/result3.png

可以发现，对于自定义的对象User的实例，进行call()运算，只能得到与普通对象类型一样的结果： [object Object] 。

## 结合三种，得到有效结果
看来我们无法找到唯一一种方法来解决所有问题，但可以结合以上三种方法的优缺点来分步判断。

比如：先使用Object.prototype.toString.call()方法，过滤出返回值为[object Object]的对象，然后再使用constructor运算，判定是否为自定义类型的对象。
