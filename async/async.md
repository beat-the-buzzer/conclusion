### Javascript异步编程

1、Generator & await/async

test1: 类似断点调试，每走一步next就接着去执行，直到下一个断点或者程序结束

	function * oddGenerator () {
	  yield 1
	  yield 3
	
	  return 5
	}
	
	let iterator = oddGenerator()
	
	let first = iterator.next()  // { value: 1, done: false }
	let second = iterator.next() // { value: 3, done: false }
	let third = iterator.next()  // { value: 5, done: true  }

test2: next带参数

	function * outputGenerator () {
	  let ret1 = yield 1
	  console.log(`got ret1: ${ret1}`)
	  let ret2 = yield 2
	  console.log(`got ret2: ${ret2}`)
	}
	let iterator = outputGenerator()
	iterator.next(1)
	iterator.next(2) // got ret1: 2
	iterator.next(3) // got ret2: 3

test3: 当作迭代器使用

	function * oddGenerator () {
	  yield 1
	  yield 3
	  yield 5
	
	  return 'won\'t be iterate'
	}
	
	for (let value of oddGenerator()) {
	  console.log(value)
	}
	// > 1
	// > 3
	// > 5
	// return 的返回不计入迭代

test4: yield* 类似 ...，作展开操作

	function * gen1 () {
	  yield 1
	  yield* gen2()
	  yield 5
	}
	function * gen2 () {
	  yield 2
	  yield 3
	  yield 4
	  return 'won\'t be iterate'
	}
	for (let value of gen1()) {
	  console.log(value)
	}
	// > 1
	// > 2
	// > 3
	// > 4
	// > 5

test5: await&async异步操作的同步化语法

	function getRandom () {
	  return new Promise(resolve => {
	    setTimeout(_ => resolve(Math.random() * 10 | 0), 1000)
	  })
	}
	
	async function main () {
	  let num1 = await getRandom();
	  console.log(num1);
	  let num2 = await getRandom()
	  console.log(num2);                               
	  return num1 + num2                           
	 }  


	// main()是一个Promise，使用await可以获取到返回的值
	console.log(`got data: ${await main()}`)

	
