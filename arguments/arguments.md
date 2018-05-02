#### arguments对象的正确使用

从一道题开始：

	function test(x,y){  
	  var x = 10;  
	  alert(arguments[0],arguments[1]);  
	} 
	test(); // arguments[0],arguments[1]都是undefined
	test(1); // arguments[0]: 1 arguments[1]: undefined 

arguments是参数对象，这个参数指的是实参，也就是调用的时候传递的参数。

1、修改arguments的方法

 - 定义同名的形参变量并修改

		function test(x,y){  
		  var x = 10;  
		  alert(arguments[0]);  
		} 
		test(1); // 10

 - 修改arguments对象

		function test(x,y){  
		  arguments[0] = 10;  
		  alert(arguments[0]);  
		} 
		test(1); //10

 - 直接修改参数

		function test(x,y){  
		  x = 10;  
		  alert(arguments[0]);  
		} 
		test(1); //10

> 综上所述，传入的参数最好不要随便修改。

2、养成正确的编程习惯

既然arguments不能随便修改，那么在写函数之前，还是需要把参数对象复制一遍。

	function test(x,y){  
	  var args = arguments;  
	  args[0] = 10;	
	  alert(arguments[0]);  
	} 
	test(1);

不好意思，上面是一个错误的示范。这个知识点涉及到了对象的浅拷贝和深拷贝。[>>>>>JS对象赋值与深拷贝、浅拷贝](http://baidu.com)

其实，如果我们经常去看一看别人的代码，就会经常发现这样的写法：

	Array.prototype.slice.call(arguments)
	[].slice.call(arguments)

这些巧妙的方法，既没有改变arguments对象，又把传入的参数变成了数组，便于操作。

或许你会疑惑，为什么arguments不设计成一个数组？哈哈，提bug提到了语言本身了。[看看知乎上面的回答](https://www.zhihu.com/question/50803453)，简单了解一下就行。

	function test(x,y){  
	  var args = Array.prototype.slice.call(arguments);  
	  args[0] = 10;	
	  alert(arguments[0]);  
	} 
	test(1);  // 1

这里的args可以认为是实参的复制品，随便怎么处理，都不影响arguments这个对象。

3、ES6的新特性

	function test(...args) {
	  args[0] = 0
	  alert(arguments[0]);
	}
	test(1, 2, 3, 4, 5); // 1

这是rest参数的用法，args是一个数组，方便操作。下面两种写法是等价的：

	// arguments变量的写法
	function sortNumbers() {
	  return Array.prototype.slice.call(arguments).sort();
	}
	
	// rest参数的写法
	const sortNumbers = (...numbers) => numbers.sort();

> 在以后的开发中，我们尽量使用优雅的ES6的实现方式，退一步，即使代码不优雅，也要使用安全、稳定的方法。