### JS对象赋值与深拷贝、浅拷贝

其实这个问题，就是js中对象、数组的深拷贝和浅拷贝。首先先说一下现象：

	const A = {
	    a: 1,
	    b: {
	        c: {
	           d: [1, 2, 3]
	        }
	    }
	};
	const B = A;

如果我修改B，那么A也会随之发生改变。

在我们平时这样对字符串、数字进行这种操作的话，B不会对A造成任何影响，但是，对象类型，是复杂的数据类型。如果我们有C语言的学习经验，就会明白“指针”这个概念。A和B都指向了同一个内存空间，share the same one.所以B改变也造成了A的改变。

	B.b.c.d[0] = 0;
	A.b.c.d[0]  // 0

JQuery的extend方法，可以设置是否进行深拷贝。

	jQuery.extend(true,A); // 深拷贝
	jQuery.extend(B); // 浅拷贝

从现实的角度出发，我们几乎不会用到这么深层次的对象。所以可以换一种方式来实现：

	const initCounties = [{
	    countryName: '中国',
	    cyCode: 'China',
	    selected: false,
	}, {
	    countryName: '美国',
	    cyCode: 'UnitedStates',
	    selected: false,
	}];

	const copyCountries = [
		...initCountries
	];

	const changeCountries = copyCountries;
	
	// 将第一条数据的selected改成true

	>>>>>浅拷贝做法
	copyCountries[0].selected = true;
	>>>>>深拷贝做法
	const change = {
		...copyCounties[0],
		selected: true
	};
	copyCountries[0] = change;

copyCountries和initCountries的指向并不完全相同，initCountries有两个坑位，copyCountries也有两个坑位，坑位是不同的，但是坑位里面的东西，可以理解成指向同一个对象的指针。浅拷贝做法是改变指针指向的内容，也就是，直接操作对象的selected属性，深拷贝是直接把整个指针换掉，坑位里换成了一个新的指针，指向了一个新的对象。

### 开发经验

上面这个数组结构在开发中其实具有普遍意义，在开发中，我们经常能看到这样的结构，在那些查询列表页面，后台返回的数据大多都是这样的结构。有人会说:直接使用jQuery的extends方法，难道还不够稳吗，还用需要这么去折腾。哈哈，如果一个项目不允许使用jQuery怎么办呢？方式一就是自己把extends方法封装一下，方式二就是用上面的方法。毕竟，虽然上面的方法有一定的局限性，但是在实际开发中，正常情况下，不会出现超多层的对象嵌套。我们在掌握理论的同时，也需要结合实际情况，写合适的代码。

