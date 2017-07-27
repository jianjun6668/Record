# promise

## promise是一个异步流程控制方法。

1. 创建一个promise

```js
let promise=function(data){
	return new Promise(function(reslove,reject){
		if(data>100){
			reslove('这个数字大于100');
		}else{
			reject('这个数字小于100');
		}
	})
}
```

2. 调用promise

```js
	promise(101)
	.then(val=>console.log(val))
	.catch(val=>console.log());

	// '这个数字大于100'

	promise(99)
	.then(val=>console.log(val))
	.catch(val=>console.log());

	// '这个数字小于于100'
```

3. promise的链式操作

```js
	promise(101).then(val=>{
		console.log(val);
		if(val%2==0){
			return '并且是一个偶数';
		}else{
			return '并且是一个奇数';
		}	
	})
	.then((val)=>console.log(val))
	.catch(val=>console.log());

	//'这是个数字大于100'
	//并且是一个奇数
```

4. 如果有错误，会跳过前面的then

```js
promise(99)
.then(val=>{
	console.log(val);
	return '下一步，这是我给你的信息'
})
.then(val=>console.log(val))
.catch(val=>console.log(val));

//'这个数字小于100'
```

5. promise是立即执行的

```js
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('Resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// Resolved
```