# Rxjs

## 创建一个servalbe流

```js
let foo=Rx.Observalbe.create(function(){
	try{
		observer.next(1);         // 通知：发出一个值，比如数字，字符串，对象等等。
		observer.Complete('end'); // 通知:发出一个js错误或者异常。
		observer.next(2);		  // 在complate后面的不会推送
	}catch(err){
		observer.Error(err);    // 通知：不发出任何值，表示流的结束。
	}	
});
```
Next通知是最重要也是最常用的类型：他代表了实际推送给Observer的值。Error和Complete通知只会在执行流中发出一次，要么是Error，要么是Complete。

在一个Observable执行流中，会发出0到无限个Next通知。而一旦Error或者Complete通知被发出，执行流将不会再推送任何消息。

## 订阅这个流

```js
foo.subscribe((x)=>{
	console.log(x); // 1 2 end
})
```

## 终止Observable流

Subscription代表了一个持续执行的过程，并且有一套最小化的API允许你中断流的执行过程。可以从这里进一步了解Subscription类型。下面例子展示了使用subscription.unsubscribe()中断持续执行的过程.

```js
var observable = Rx.Observable.from([10, 20, 30]);
var subscription = observable.subscribe(x => console.log(x));
// Later:
subscription.unsubscribe();
```

多个Subscription可以被组合在一起，从而使调用其中一个Subscription的unsubscribe()方法能够让所有的Subscription都取消流的执行。要做到这一点，可以将一个subscription实例“添加”到另一个中去.

```js
var observable1 = Rx.Observable.interval(400);
var observable2 = Rx.Observable.interval(300);

var subscription = observable1.subscribe(x => console.log('first: ' + x));
var childSubscription = observable2.subscribe(x => console.log('second: ' + x));

subscription.add(childSubscription);

setTimeout(() => {
  // Unsubscribes BOTH subscription and childSubscription
  subscription.unsubscribe();
}, 1000);

// second: 0
// first: 0
// second: 1
// first: 1
// second: 2
```

Subscription也有一个名为remove(otherSubscription)的方法，用来撤销已经添加到其中的其他Subscription。

## Subject

每一个Subject就是一个Observable流，同时也是一个观察者(Observer)。

```js

var subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

subject.next(1);
subject.next(2);

// observerA: 1
// observerB: 1
// observerA: 2
// observerB: 2

```

因为Subject是一个Observer，因此你也可以将它作为任何Observable的subscribe()的参数，订阅这个Observable流，就像下面这样：

```js

var subject = new Rx.Subject();

subject.subscribe({
  next: (v) => console.log('observerA: ' + v)
});
subject.subscribe({
  next: (v) => console.log('observerB: ' + v)
});

var observable = Rx.Observable.from([1, 2, 3]);

observable.subscribe(subject); 

// observerA: 1
// observerB: 1
// observerA: 2
// observerB: 2
// observerA: 3
// observerB: 3

```


