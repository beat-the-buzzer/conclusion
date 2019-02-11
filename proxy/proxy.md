### ES3/ES5/ES6保护对象的熟悉

##### ES3——使用闭包

	{
		// ES3,ES5 数据保护
		var Person = function() {
			var data = {
				name: 'es3',
				sex: 'male',
				age: 15
			}
			this.get = function(key) {
				return data[key]
			}
			this.set = function(key, value) {
				if (key !== 'sex') {
					data[key] = value
				}
			}
		}
	
		var person = new Person();
		// 读取
		console.table({
			name: person.get('name'),
			sex: person.get('sex'),
			age: person.get('age')
		});
		// 修改
		person.set('name', 'es3-cname');
		console.table({
			name: person.get('name'),
			sex: person.get('sex'),
			age: person.get('age')
		});
		person.set('sex', 'female');
		console.table({
			name: person.get('name'),
			sex: person.get('sex'),
			age: person.get('age')
		});
		// 修改失败
	}

ES3的做法就是给`Person`对象新增API去访问属性，但是我们不能直接去访问对象的属性。我们可以在`get`和`set`方法中添加相应的逻辑。

##### ES5——使用defineProperty保护数据

	{
		// ES5
		var Person = {
			name: 'es5',
			age: 15
		};
	
		Object.defineProperty(Person, 'sex', {
			writable: false,
			value: 'male'
		});
	
		console.table({
			name: Person.name,
			age: Person.age,
			sex: Person.sex
		});
		Person.name = 'es5-cname';
		console.table({
			name: Person.name,
			age: Person.age,
			sex: Person.sex
		});
		try {
			Person.sex = 'female';
			console.table({
				name: Person.name,
				age: Person.age,
				sex: Person.sex
			});
		} catch (e) {
			console.log(e);
		}
	}

使用`Object.defineProperty`设置sex属性为不可写，这样如果给sex赋值，就会报错，这样就实现了对象属性保护。

##### ES6对象代理

	{
		// ES6
		let Person = {
			name: 'es6',
			sex: 'male',
			age: 15
		};
	
		let person = new Proxy(Person, {
			get(target, key) {
				return target[key]
			},
			set(target, key, value) {
				if (key !== 'sex') {
					target[key] = value;
				} else {
					throw 'Sex cannot be evaluated!'
				}
			}
		});
	
		console.table({
			name: person.name,
			sex: person.sex,
			age: person.age
		});
	
		try {
			person.sex = 'female';
		} catch (e) {
			console.log(e);
		}
	}

`Proxy`是ES6新增的特性。

 - get方法：访问属性，get方法的两个参数指的是当前代理对象和键名。显然，我们可以拦截，例如，年龄保密，可以在get方法里这样写：

		get(target, key) {
			if(key === 'age') {
				return '***';
			}
			return target[key]
		},

 - set方法：给属性赋值，set方法的三个参数分别是当前代理对象，键名和键值。上面的例子里，如果我们对性别赋值，就会抛出一个异常。

我们可以在get和set方法中写大量的业务逻辑，可以保护原对象。