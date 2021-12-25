# JavaScript Learning Notes

Author: Zher Leon

Last Update: Aug 29, 2021

---

### ⏳ 类 - Class

- 声明

  使用带有class关键字的类名进行声明。

``` 
class Cat() {
	constructor(name,color) {
		this.name = name;
		this.color = color;
	}
}
```

- 提升

  类声明与函数声明不一样，函数声明会提升，而类说明不会。所以，在使用的时候，需要先声明类然后再访问它，否则将会抛出ReferenceError的错误喔。
``` 
错误使用：
let meow = new Cat();
class Cat {}

正确使用：
class Cat {}
let meow = new Cat();
```

  （提升-Hoisting：可以理解为，变量和函数的声明在物理层面上移动到代码的最前面，虽然实际上变量和函数声明在代码中是不会动的，而是在编译阶段被放入内存中。）

- 类表达式

  类表达式时定义类的另一种方法。类表达式可以命名或不命名。表达式的名称是该类的局部名称。
```
let cat = class {
	constructor(name,color) {
		this.name = name;
		this.color = color;
	}
}
console.log(cat.name) ==>> cat

let cat = class catEmulator {
	constructor(name,color) {
		this.name = name;
		this.color = color;
	}
}
console.log(cat.name) ===> catEmulator

```

- 类体和方法定义

  一个类的类体是指花括号中的部分，里面可以定义方法和构造函数。

`构造函数`

​	constructor用于创建和初始化一个由class创建的对象。一个类只能拥有一个constructor。如果多个，则会抛出SyntaxError。

​	一个构造函数可以使用super关键字来调用一个父类的构造函数。

`原型方法`
```
class Cat {
	// constructor
	constructor(name, color) {
		this.name = name;
		this.color = color;
	}
	// Getter
	get catInfo() {
		return this.getPetcat();
	}
	// Method
	getPetCat() {
		return `从前有一只叫${name}的小猫咪，它有一身${color}的毛发`
	}
}
const meow = new Cat(‘旺财’,'金色');

console.log(meow.catInfo());
// 从前有一只叫旺财的小猫咪，它有一身金色的毛发
```

`静态方法`

​	static关键字用于定义一个类的静态方法，调用静态方法不需要实例化该类，但不能通过一个类实例调用静态方法，通常用于一个应用程序创建工具函数。

```
class Cat {
	// constructor
	constructor(name, color) {
		this.name = name;
		this.color = color;
	}
	static displayName = 'A story about a cat.';
	static catInfo(name,color) {
		return `从前有一只叫${name}的小猫咪，它有一身${color}的毛发`
	}
}
// 不能通过一个类的实例调用
const meow = new Cat('旺财','金色')
const discrption = meow.displayName; // undefined
const info = meow.catInfo(); // undefined
// 但是可以通过该类去调用
console.log(Cat.diaplayName); // A story about a cat.
console.log(Cat.catInfo('旺财',’金色‘));  // 从前有一只叫旺财的小猫咪，它有一身金色的毛发
```

### ⏳ Array filter()

​	filter()用于对**数组**进行过滤。它的结果会创建一个新的数组，新数组中的元素时通过检查的指定数组中符合条件的所有元素。

​	比如说：

![Array Filter](JavaScript-LearningNotes.assets/image-20210830160255503.png)

​	注意：过滤符合条件的元素，创建新数组进行存储，不会改变原本的数组。

### ⏳ Symbol

​	ES6引入的一个新的数据类型。可以在全局生成唯一的一个标识值。

### ⏳ 操作符

​	最常用的操作符有：=、-、*、/、%、+=、-= 等等。下面记录一下我不熟悉的一些。

#### 		🔗 可选链操作符（?.）

​	允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。?.和.不同之处在于当引用是空 (null 或者 undefined)的情况下不会引起报错，如果是空的，这个表达式的短路返回值是`undefined`

```
const myInfo = {
	name: 'Tom',
	cat: {
		name: 'milk'
	}
}
const dogName = myInfo.dog.name; 
//Reset Error: Cannot read properties of undefined (reading 'name')
const mydog = myInfo.dog?.name;
// undefined
```

​	⚠ 不要去尝试对所有事物进行可选链接。虽然可选链很好用，但并不是所有在 ECMA 下的东西都可以选择链接在一起。基本上，只需记住 3 种受支持的操作：属性访问器，数组和函数调用。

下面是不支持的操作列表：

```
// Delete
delete a.?b   // ReferenceError: Can't find variable: a

// Constructor
new a?.()     // SyntaxError: Cannot call constructor in an optional chain.

// Template literals
a?.`string`   // SyntaxError: Cannot use tagged templates in an optional chain.

// Property assignment
a?.b = c      // SyntaxError: Left side of assignment is not a reference.

// Imports, Exports, Super, etc. etc.
```

#### 	🔗 空值合并运算符 (??)

​	**空值合并操作符**（**`??`**）是一个逻辑操作符，当左侧的操作数为 `null` 或者 `undefined`时，返回其右侧操作数，否则返回左侧操作数。

```
const mycat = null ?? 'hello'
// 'hello'
const mycat = 0 ?? 'hello'
// 0
如果mycat是null或者undefined就返回右边的值，如果不是就返回左边的值。
```

​	⚠需要注意，不要和&&或者||直接连接使用，因为??和它们的运算优先级或者说是运算顺序还没有被定义。

```
null || undefined ?? "foo"  //抛出SyntaxError
true || undefined ?? "foo"  //抛出SyntaxError

// 但是加上括号来显式表明运算优先级，是没有问题的.
(null || undefined) ?? "foo" //返回“foo"
```

#### 	🔗 逻辑空赋值(??=)

​	逻辑空赋值运算符 (`x ??= y`) 仅在 `x` 是 nullish (`null` 或 `undefined`) 时对其赋值。

```
function config(options) {
	options.duration ??= 100;
	options.speed ??= 25;
	return options;
}

config({duration: 125}); 
//output: { duration: 125, speed: 25 }
config({});
//output: { duration: 100, speed: 25 }

第一个传入了duration, 所以不需要赋值，但是没有传入speed，则speed为空，所以返回speed为25，第二个同样道理，都没传，所以返回了默认值。
```

#### 	🔗 delete 操作符

​	用于删除对象的某个属性，如果没有指向这个属性的引用，那它就会被释放。

```
const Employee = {
  firstname: 'John',
  lastname: 'Doe'
};

console.log(Employee.firstname);
// expected output: "John"

delete Employee.firstname;

console.log(Employee.firstname);
// expected output: undefined
```



### ⏳ GC-垃圾回收机制

​	GC(Garbage Collection)，中文称为垃圾回收机制。在程序中，GC的作用是回收不再使用的内存空间。对于C语言这种高级语言，提供了malloc()或者free()这样的API要求程序员显示的分配或者释放内存，这种就是要手动实现内存管理，如果不小心释放了正在使用中的内存就会出BUG。

​	**然而JavaScript是一门宽容的语言。**JavaScript的实现已经搭载好了GC。通过GC，前端把内存管理交给计算机，不需要手动释放内存。虽然是这样，但也不能忽略内存管理，否则同样会引起内存的泄露，引起交互性。

参考链接：https://blog.csdn.net/szengtal/article/details/99488127



### ⏳ 享元模式

​	享元(Flyweight)模式，用于性能优化。

​	假设有个内衣工厂，目前的产品有50种男式内衣和50种女士内衣，为了推销产品，工厂决定生产一些塑料模特来穿上他们的内衣拍成广告照片。正常情况下需要50个男模特和50个女模特，然后让他们每人分别穿上一件内衣来拍照。不使用享元模式的情况下，在程序里也许会这样写：

```
var Model = function(sex, underwear) {
	this.sex = sex;
	this.underwear = underwear;
}
Model.prototype.takePhoto = function() {
	console.log(`sex=${this.sex}, underwear=${this.underwear}`);
}
for (var i = 1; i<=50; i++) {
	var maleModel = new Model('male', 'No.'+i);
	maleModel.takePhoto();
}
for (var i = 1; i<=50; i++) {
	var femaleModel = new Model('female', 'No.'+i);
	femaleModel.takePhoto();
}
```

​	拍一张照片每次都要传入sex和underwear参数，如果各有50种内衣，就需要产生一百个对象，如果将来生产10000种，那么程序可能会因为存储过多的对象而提前崩溃。

​	为了优化这个场景，换个角度想，虽然有一百种内衣，但是很显然不需要男女各五十个模特，只需要一个男模特和一个女模特，让他们分别穿上不同的内衣拍照即可。这个时候可以这么写：

```
var Model = function(sex) {
	this.sex = sex;
}
Model.prototype.takePhoto = function() {
	console.log(`sex=${this.sex}, underwear=${this.underwear}`);
}
var maleModel = new Model('male')
var femaleModel = new Model('female')
for( var i =1; i<=50; i++) {
	maleModel.underwear = 'underwear' + i;
	maleModel.takePhoto();
}
for( var j=1; j<=50; j++) {
	femaleModel.underwear = 'underwear' + j;
	femaleModel.takePhoto();
}
```

​		这两段乍一看就是将创建对象这个步骤从内部变量提取成外部变量。享元模式的目标就是尽量减少共享对象的数量。

​	对于如何划分内部和外部状态，可以根据下面几条：

	- 内部状态存储于对象内部
	- 内部状态可以被一些对象共享
	- 内部状态独立于具体的场景，通常不会改变
	- 外部状态取决于具体的场景，并根据场景而变化，外部状态不能被共享

### ⏳ Babel

​	Babel[ˈbeɪbl]，是一个用于编写下一代JavaScript的编译器，当我们使用了JavaScript最新版本的语法时，Babel可以将新语法转化为ES5，让低端环境运行（因为ES5规范覆盖了绝大部分的浏览器，所以转换到ES5是个安全且流行的做法啦。）



### ⏳ Set对象

​	Set对象是唯一值的集合，每个值在Set中只能出现一次。一个Set可以容纳任何数据类型的任何值。

```
// 合并数据
const a = [1,2,3]
const b = [1,4,5]
const obj1 = { a: 1}
const obj2 = { b: 1}

// 不合理的写法
const c =  a.concat(b); //[1,2,3,1,4,5]
const obj = Object.assign({}, obj1, obj2) // {a:1, b:1}

// 改进
const c = [...new Set([...a,...b])]; //[1,2,3,4,5]
const obj = {...obj1, ...obj2}; //{a:1, b:1}

```



### ⏳ 关于${}

​		在`${}`中可以放入任意的JavaScript表达式，可以进行运算，以及引用对象属性。

```
const name = 'Tom'
const score = 59
let result = ''
if(score > 60) {
	result = `${name}的考试成绩及格`
}else {
	result = `${name}的考试成绩不及格`
}

其实可以这么写:
const result = `${name}${score > 60 ? '的考试成绩及格':'的考试成绩不及格'}`
```

关于一些写法的改进可以继续查看： [你会用ES6，那倒是用啊！](https://juejin.cn/post/7016520448204603423)



### ⏳ 关于ES5、ES6和ES2015

​	ES2015特指在2015年发布的新一代JS语言标准，ES6泛指下一代JS语言标准，包括ES2015、ES2016、ES2017、ES2018等。现阶段绝大部分场景下，ES2015默认等同于ES6。ES5泛指上一代语言标准，ES2015可以理解为ES5和ES6的时间分界线。



### ⏳ 关于this的指向问题

​	当函数在调用时，会创建一个执行上下文，这个上下文包括函数调用的一些信息（调用栈、传入参数、调用方式），this就是指向这个执行上下文。

> 注意： this不是静态的，也不是编写时绑定的，是在 **运行时**绑定的，它的绑定和函数声明的位置没有关系，仅仅取决于函数调用的方式。

#### 	🔗 this 指向

​		在js中，this的绑定规则一共有5种：

1. 默认绑定
2. 隐式绑定
3. 显式绑定（硬绑定）
4. new 绑定
5. ES6箭头绑定

- 默认绑定

  指函数独立调用，不涉及其他绑定规则。在非严格模式下，`this`指向window，严格模式下， `this`指向undefined

  >  非严格模式

  ```javascript
  var foo = 123
  function print() {
  	this.foo = 234
  	console.log(this) //window
  	console.log(foo) //234
  }
  print()
  ```

  非严格模式下，print()是默认绑定，`this`指向window，所以打印window和234。至于这个foo，在预编译的过程中，foo和print函数会存放在全局`GO`中(即window对象上)，所以上面的foo相当于window.foo。

  > 严格模式

  ```javascript
  "use strict" //开启严格模式
  var foo = 123
  function print() {
  	console.log('print this is',this)  //undefined
  	console.log(window.foo) // 123 
  	console.log(this.foo)  // 报错：Uncaught TypeError: Cannot read property 'foo' of undefined
  }
  console.log('global this is', this)  // window
  print()
  ```

  开启严格模式后，函数内部的`this`会指向undefined，但全局对象window不会受影响。

  > 使用let/const

  ```javascript
  let a = 1
  const b = 2
  var c = 3
  function print() {
  	console.log(this.a)  // undefined
  	console.log(this.b)  // undefined
  	console.log(this.c)  // 3
  }
  print()
  console.log(this.a) // undefined
  ```

  let和const定义的变量存在暂时性死区，而且不会挂在到window上，所以效果如上。

  > 在对象内执行

  ```
  a = 1
  function foo() {
  	console.log(this.a)
  }
  const obj = {
  	a: 10
  	bar() {
  		foo()  //1
  	}
  }
  obj.bar();
  ```

  foo虽然在obj的bar函数中，但是foo的函数仍然时独立运行的，所以，foo中的`this`依然指向window对象。

  > 在函数内执行

  ```
  a = 1；
  (function() {
  	console.log(this)  // window
  	console.log(this.a) // 1
  })
  function bar() {
  	b = 2  //全局
  	(function() {
  		console.log(this)  //window
  		console.log(this.b)  // 2
  	})
  }
  bar()
  ```

  自执行函数只要执行到就会运行，且只运行一次，`this`指向window。

  #### 🔗 隐式绑定

  隐式绑定就是，函数的调用是在某个对象上触发的，即调用位置存在上下文对象，大概就是 **XXX.func()** 这种调用模式。

  此时`func`的this指向`XXX`，但如果存在链式调用，例如XXX.YYY.ZZZ.func，记住一个原则： **this永远指向最后调用它的那个对象**。

  > 隐式绑定  

  ```
  var a = 1;
  function foo() {
  	console.log(this.a)
  }
  var obj = {a: 2, foo}
  foo()  // 1, 默认绑定
  obj.foo() // 2, 隐式绑定
  ```

  `obj`是由var定义的, 因此会挂在到window, 所以obj.foo()相当于window.obj.foo(), foo中的this指向obj的this.

  > 对象链式调用

  ```
  var obj1 = {
  	a: 1,
  	obj2: {
  		a: 2,
  		foo() {
  			console.log(this.a)
  		}
  	}
  }
  obj1.obj2.foo() // 2
  ```

  ####  🔗 隐式绑定丢失

  ​	隐式绑定的丢失一般有两种方式：

     - 用另一个变量作为函数名，之后再用这个别名执行函数

     - 将函数作为参数传递时会隐式赋值

       丢失之后，`this`的指向会启用默认绑定。

       > 取函数别名

       ```
       // 函数别名在全局
       a = 1
       var obj = {
       	a: 2,
       	foo() {
       		console.log(this.a)
       	}
       }
       var foo = obj.foo
       obj.foo()  // 2
       foo()  // 1
       ```

       `JavaScript`对于引用类型，其地址指针存放在栈内存中，真正的本体是存放在堆内存中的。

       上面将`obj.foo`赋值给`foo`，就是将`foo`也指向了`obj.foo`所指向的堆内存，此后再执行`foo`，相当于直接执行的堆内存的函数，与`obj`无关，`foo`为默认绑定。笼统的记，**只要fn前面什么都没有，肯定不是隐式绑定**。

       ```
       // 函数别名在对象中
       var obj = { 
           a: 1, 
           foo() {
               console.log(this.a)
           } 
       };
       var a = 2;
       var foo = obj.foo;
       var obj2 = { a: 3, foo: obj.foo }
       
       obj.foo(); // 1
       foo(); // 2
       obj2.foo(); // 3
       ```

       `obj2.foo`指向了`obj.foo`的堆内存，此后执行与`obj`无关(除非使用`call/apply`改变`this`指向)

       > 函数作为参数传递

       ```
       function foo() {
         console.log(this.a)
       }
       function doFoo(fn) {
         console.log(this)
         fn()
       }
       var obj = { a: 1, foo }
       var a = 2
       doFoo(obj.foo)  // Window{...} 2
       ```

       `obj.foo`作为实参，在预编译时将其值赋值给形参`fn`，是将`obj.foo`指向的地址赋给了`fn`，此后`fn`执行不会与`obj`产生任何关系。`fn`为默认绑定。

