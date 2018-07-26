### 类型检测

一、内置类型和typeof方法

JavaScript有7种内置类型:

 - 空值（null）
 - 未定义（undefined）
 - 布尔值（boolean）
 - 数字（number）
 - 字符串（string）
 - 对象（object）
 - 符号（symbol，ES6新增类型）

我们可以使用typeof运算符来查看值的类型：

	typeof undefined === 'undefined'
	typeof true === 'boolean'
	typeof 2 === 'number'
	typeof '2' === 'string'
	typeof {name:'James'} === 'object'
	typeof Symbol() === 'symbol'

有一个特殊的地方，就是null

	typeof null === 'object'

这个设定是我们无法改变的，我们能做的事情就是，判断一个值是否是null的时候，要进行复合判断：

	var a = null;
	(!a && typeof a === 'object')

还有一些其他的情况：

	typeof function a(){} === 'function'
	typeof [1,2] === 'object'

特别注意：undefined 和 is not defined

	var a;
	typeof a; // undefined

	b; // ReferenceError: b is not defined
	typeof b; // undefined

这里有一件很诡异的事情：b没有定义，但是可以拿到typeof的值。这是因为typeof的安全机制。

二、JavaScript中的值

1、数组和类数组

JavaScript中的数组

JavaScript中的数组是自由的，可以容纳任何类型的值，数组不需要声明它的长度，可以直接向里面填值。

类数组的意思是一些对象有数组的性质，有长度有索引，但是它们不是数组，例如DOM对象，arguments对象。我们经常使用slice方法将其转换成真正的数组，或者使用ES6的Array.from方法：

	Array.prototype.slice.call(arguments);
	[].slice.call(arguments);
	Array.from(arguments);

2、字符串

字符串是不可变的，也就是说，字符串调用它的成员函数不会改变字符串本身。

	var a = 'foo';
	[].join.call(a, '-'); // f-o-o

数组的reverse方法不能用在字符串上，原因是：数组的reverse方法是直接把数组自身翻转。

	var arr = [1,2,3,4];
	arr.reverse();
	arr; // [4,3,2,1]

但是，我们不能使用下面的方法来实现字符串的翻转：

	var a = '1234';
	Array.prototype.reverse.call(a)

正确的做法应该是这样：

	var c = a.split('').reverse().join('');

这种方法对复杂字符无效，例如Unicode。

3、数字

JavaScript中的数字类型是一个容易出坑的问题。JavaScript中的数字类型是基于IEEE 754的标准来实现的。

下面几种写法都是合法的：

	var a = 42;
	var b = 42.3;
	var a1 = 0.42;
	var b2 = .42;
	var a3 = 42.0;
	var b3 = 42.;

数字类型都使用了Number对象进行了封装，因此数字值可以使用Number.prototype上面的方法，例如toFixed方法可以指定小数位,toPrecision方法可以指定有效位数。

	var a = 42.59;
	a.toFixed(0); // 字符串 '42'
	a.toFixed(3); // 字符串 '42.590'
	a.toPrecision(1);

注意：有些时候会出现无效的语法，例如：

	42.toFixed(2); // SyntaxError

这里的42. 是一个整体，谁让JavaScript认为42. 是一个合法的数字呢，正确的做法是这样：

	42..toFixed(2); // 42.00,
	(42).toFixed(2);  // 42.00

整数检测：ES6的Number.isInteger方法

	Number.isInteger(42); // true
	Number.isInteger(42.0); // true
	Number.isInteger(4.2); // false

安全整数检测：ES6的Number.isSafeInteger方法

整数的最大值是2^53-1，也就是Number.MAX_SAFE_INTEGER。

	Number.isSafeInteger(Number.MAX_SAFE_INTEGER); // true
	Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1); // false
	Number.isSafeInteger(Number.MAX_SAFE_INTEGER - 1); // true

4、null 和 undefined

 - null指的是空值（empty value）或者曾经赋过值，现在没有值
 - undefined指的是没有值（missing value）或者没有被赋过值

void运算符可以得到undefined，不过我们不提倡这种方法，eslint中有关于void的规则：

	Disallow use of the void operator. (no-void)

5、NaN

NaN，意思是not a number，但是奇怪的是，typeof NaN 的值是'number'。

	var a = 2 / 'str'; // NaN
	typeof NaN === 'number' // true

更诡异的是，NaN和它本身并不相等，如果检测一个值是否是NaN，不能这样：

	var a = 2 / 'str'; // NaN
	a == NaN;  // false
	a === NaN; // false

我们可能都知道系统自带一个isNaN方法，可以检测一个值是否是NaN，但是这个方法有点问题：

	var a = 2 / 'str';
	var b = 'str';
	a; // NaN
	b; // 'str'
	window.isNaN(a);  // true
	window.isNaN(b);  // true !!!

很显然，对b的检测不是我们想要的结果。好在ES6提供了更好的Number.isNaN方法来进行检测：
	
	// polyfill
	if(!Number.isNaN){
		Number.isNaN = function(n) {
			return (
				typeof n === 'number' &&
				window.isNaN(n)
			)
		}
	}

实际上，我们还有更快的方法：

	if(!Number.isNaN) {
		Number.isNaN = function(n) {
			return n !== n
		}
	}

为了程序的稳定性和健壮性，我们尽量使用Number.isNaN方法。

6、值和引用

这一部分的内容在这篇文章里略有提及，就是深拷贝浅拷贝的问题。

[https://github.com/beat-the-buzzer/conclusion/blob/master/copyArrayOrObj/copyArrayOrObj.md](https://github.com/beat-the-buzzer/conclusion/blob/master/copyArrayOrObj/copyArrayOrObj.md)

> JavaScript中的数组是通过数字索引的一组任意类型的值。字符串和数组类似，但是它们的行为特征不同，字符call/apply数组方法的时候要小心。




	
 
