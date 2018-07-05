### 构造函数、原型、原型链

> 封装函数其实是封装一些公共的逻辑和功能，通过传入参数的形式达到自定义的效果。当面对具有共同特征的一类事物时，就可以结合构造函数与原型的方式将这类事物封装成对象。

例如：

	// 构造函数
	var Person = function(name, age) {
		this.name = name;
		this.age = age;
	}
	// Person.prototype为Person的原型
	Person.prototype.getName = function() {
		return this.name;
	}

这样，我们就利用构造函数与原型封装好了一个Person对象。

**具体某一个人的特定属性，通常放在构造函数中。**例如此处的name、age，它们的值不是所有人的公共属性，而是仅仅属于某一个人。

**所有公共的方法与属性，通常都会放在原型对象中。**例如此处的getName，它表示一个共同的动作，访问当前这个人的属性。

当我们想要使用Person对象创建一个具体的“人”时，我们称这个被创建的“人”为一个**实例**。

	var p1 = new Person('Tom',23);
	var p2 = new Person('Jerry',20);

	p1.getName(); // Tom
	p2.getName(); // Jerry

p1和p2都是根据对象Person创建的实例。我们可以通过原型的方法访问该实例的属性。

**构造函数的this，和原型方法里面的this，指向当前的实例。**为什么？我们需要探讨一下，new关键字到底做了什么事情。

	function New(func) {
		// 声明一个中间对象，该对象为最终返回的实例
		var res = {};
		if(func.prototype !== null) {
			// 将实例的原型指向构造函数的原型
			res.__proto__  = func.prototype;
		}
	
		// ret为构造函数的执行结果
		// 将构造函数内部的this指向修改为指向res 
		// 通过apply实现
		var ret = func.apply(res, [].slice.call(arguments, 1));
	
		// 当我们在构造函数中指定了返回的对象时，new执行的结果就是这个对象
		if((typeof ret === 'object' || typeof ret === 'function') && ret !== null) {
			return ret;
		} 
	
		// 默认返回res
		return res;
	}
	
	var Person = function(name) {
		this.name = name;
	}
	
	Person.prototype.getName = function() {
		return this.name;
	}
	
	var p1 = New(Person, 'Tom');
	var p2 = New(Person, 'Jerry');
	
	p1.getName(); // Tom
	p2.getName(); // Jerry

使用New方法声明的实例与new关键字声明的实例有相同的特性与效果。

new 关键字在创建实例的时候经历了如下过程：

1. 先创建一个新的、空的实例对象
2. 将实例对象的原型指向构造函数的原型
3. 将构造函数内部的this，修改为指向实例
4. 返回实例对象

PPrototype指代原型对象：

	person.prototype -> PPrototype
	p1.__proto__ -> PPrototype
	p2.__proto__ -> PPrototype
	PPrototype.constructor -> Person

![https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/conclusion/prototype-01.png](https://raw.githubusercontent.com/beat-the-buzzer/pictures/master/conclusion/prototype-01.png)

从前面的分析可以看出，构造函数的prototype与所有实例的\_\_proto\_\_都指向原型对象，而原型对象的constructor指向构造函数。

因为在构造函数中声明的变量与方法只属于当前实例，因此我们可以将构造函数中声明的属性与方法称为该实例的**私有属性**与**私有方法**，它们只能被当前实例访问。而原型中的方法与属性能够被所有的实例访问，因此我们将原型中声明的属性与方法称为**公有属性**与**公有方法**。

当在构造函数中声明一个方法时，每创建一个实例，改方法都会被重新创建一次。而原型中的方法仅仅只会被创建一次。因此在构造函数中，声明私有方法会消耗更多的内存空间。

如果构造函数中声明的私有方法/属性与原型中的公有方法/属性重名，那么会优先访问私有方法/属性。

	function Person(name) {
		this.name = name;
		this.getName = function() {
			return this.name + ', 你正在访问私有方法';
		}
	}
	
	Person.prototype.getName = function() {
		return this.name;
	}
	
	var p1 = new Person('Tom', 20);
	p1.getName(); // Tom, 你正在访问私有方法

可以通过in方法来判断一个对象是否拥有某一个方法或属性。

	console.log('name' in p1); // true 
	console.log('age' in p1); // false
	console.log('getName' in p1); // true

我们可以通过in来判断当前环境是否是移动端环境

	var isMobile = 'ontouchstart' in document;

我们可以通过in来遍历对象，通常情况下，我们需要把原型链上的方法过滤掉。

	var obj = {
		'name': 'Tom',
		'age': 20
	};
	
	for(var i in obj) {
		if(obj.hasOwnProperty(i)) {
			console.log(i + ': ' + obj[i]);
		}
	}

除了上面的在原型上添加方法之外，还有其他的方式：

	Person.prototype = {
		constructor: Person,
		getName: function() {},
		getAge: function() {},
		sayHello: function() {}
	}

我们在这里把一个新的对象直接赋值给prototype，如果不将它的constructor指向Person，就会导致原型对象的丢失。

1. 当一个对象A作为原型时，它有一个constructor属性指向它的构造函数；
2. 当一个对象B作为构造函数时，它有一个prototype属性指向它的原型；
3. 当一个对象C作为实例时，它有一个\_\_proto\_\_属性指向它的原型；

判断一个对象foo是否是构造函数Foo的实例时，可以使用instanceof关键字，返回一个boolean值。

	foo instanceof Foo

创建一个对象的时候，可以通过new Object()来创建。因此Object实际上是一个构造函数，Object.prototype是原型链的终点。

所有的函数和对象都有一个toString()和valueOf()方法，就是来自于Object.prototype

原型链是实现继承的理论基础。

	function Person(name, age) {
		this.name = name;
		this.age = age;
	}

	Person.prototype.getName = function() {
		return this.name;
	}

	Person.prototype.getAge = function() {
		return this.age;
	}

	function Student(name, age, grade) {
		// 构造函数继承
		Person.call(this, name, age);
		this.grade = grade;
	}

	// 原型继承
	Student.prototype = Object.create(Person.prototype, {
		// 这里需要指定constructor
		constructor: {
			value: Student
		},
		getGrade: {
			value: function() {
				return this.grade
			}
		}
	});	

	var s1 = new Student('Ming', 22, 5);
	console.log(s1.getName()); // Ming
	console.log(s1.getAge()); // 22
	console.log(s1.getGrade()); // 5

上面实现的原型继承使用的是defineProperties的方法，事实上，这个方法里不仅仅只有value一个属性：

1. configurable: 表示该属性能否被delete删除，默认值是true
2. enumerable： 是否能被枚举，也就是：是否能被for-in遍历
3. writable: 是否能修改，默认为true
4. value: 该属性的值
5. get/set: 通过person.name访问的时候，get方法会被调用，设置person.name = 'Jack'的时候，set方法将被调用。

如果是设置单一的属性，就使用defineProperty方法。

使用Object.getOwnPropertyDescriptor方法可以读取属性的特性值。

