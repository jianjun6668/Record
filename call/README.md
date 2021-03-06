# js中的call()用法

## 方法定义

### * call方法: 

语法：call([thisObj[,arg1[, arg2[,   [,.argN]]]]]) 
定义：调用一个对象的一个方法，以另一个对象替换当前对象。 
说明： 
call 方法可以用来代替另一个对象调用一个方法。call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。 
如果没有提供 thisObj 参数，那么 Global 对象被用作 thisObj。 

### * apply方法： 

语法：apply([thisObj[,argArray]]) 
定义：应用某一对象的一个方法，用另一个对象替换当前对象。 
说明： 
如果 argArray 不是一个有效的数组或者不是 arguments 对象，那么将导致一个 TypeError。 
如果没有提供 argArray 和 thisObj 任何一个参数，那么 Global 对象将被用作 thisObj， 并且无法被传递任何参数。
 
## 常用实例

1. 普通用法

```js
function add(a,b){
	alert(a+b);
}
function sub(a,b){
	alert(a-b);
}

add.call(sub,4,3); // 7
 ``` 

 这个例子中的意思就是用 add 来替换 sub，add.call(sub,3,1) == add(3,1) ，所以运行结果为：alert(4); // 注意：js 中的函数其实是对象，函数名是对 Function 对象的引用。
 
2. 乾坤大挪移

```js
function A(){
	this.name='spring';
	this.showName=function(a){
		alert(this.name);
	}
}
function B(){
	this.name='new Spring';
}

let a=new A();
let b=new B();

a.showName.call(b,'11111'); // new Spring 
 ```  

 call 的意思是把 A 的方法放到cat上执行，原来B是没有showName() 方法，现在是把A 的showName()方法放到 B上来执行，所以this.name 应该是 Cat
 
3. 实现继承

```js
function Animal(name){      
    this.name = name;      
    this.showName = function(){      
        alert(this.name);      
    }      
}      
   
function Cat(name){    
    Animal.call(this, name); 
}      
    
var cat = new Cat("Black Cat");     
cat.showName();  // Black Cat
```

 Animal.call(this) 的意思就是把 Animal对象的属性加到this对象，那么 Cat中不就有Animal的所有属性和方法了吗，Cat对象就能够直接调用Animal的方法以及属性了.
 
d、多重继承

```js
function Class1(){  
    this.showSub = function(a,b){  
        alert(a-b);  
    }  
}  
  
function Class2(){  
    this.showAdd = function(a,b){  
        alert(a+b);  
    }  
}  
  
function Class3(){  
    Class1.call(this);  
    Class2.call(this);  
} 
var a=new Class3();
console.log(a)  
```
![](https://github.com/jianjun6668/Record/tree/master/img/4.jpg)  


 很简单，使用两个 call 就实现多重继承了
当然，js的继承还有其他方法，例如使用原型链，这个不属于本文的范畴，只是在此说明call 的用法。说了call ，当然还有 apply，这两个方法基本上是一个意思，区别在于 call 的第二个参数可以是任意类型，而apply的第二个参数必须是数组，也可以是arguments
还有 callee，caller..
