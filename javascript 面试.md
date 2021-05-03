# 1. 语言基础

## 1.1 变量

### 1.1.1 变量声明

------

#### 1.1.1.1 let 和 const 的注意点？

- 1.声明的变量只在声明时的代码块内有效
- 2.不存在声明提升
- 3.存在暂时性死区，如果在变量声明前使用，会报错
- 4.不允许重复声明，重复声明会报错
------

### 1.1.2 变量提升

#### 1.1.2.1 变量提升与函数提升有何区别？

+ 针对变量的声明提升（var声明），只有变量的**定义声明**才会被提升，变量的**赋值声明**不会被提升。

```js
console.log(a); // 2
var a = 2;
```

+ 针对函数的声明提升（直接的函数声明和函数表达式声明），函数声明会被提升，函数表达式（等号后）不会被提升。

```js
foo();
function foo(){
	console.log(a); // undefined
	var a=2;
}

// 提升后，同

function foo(){
	var a;
	console.log(a);
	a=2;
}
foo()
```

+ 函数声明优先于变量声明而提升(函数声明会被提升到普通变量之前），命名重复的声明会被忽略。

```js
foo(); //1 
var foo;
function foo(){
	console.log(1);
}
foo=function(){ // 函数表达式
	console.log(2);
}
// 相当于
```
```js
function foo(){
	console.log(1);
};
// var foo 命名相同被忽略
foo();
foo=function(){
	console.log(2);
}
```

> 没有用var 声明的变量不会提升。

------
#### 1.1.2.2 [面试题1]一道常被人轻视的前端 JS 面试题
```js
function Foo() {
  getName = function() {
    alert(1);
  };
  return this;
}
Foo.getName = function() {
  alert(2);
};
Foo.prototype.getName = function() {
  alert(3);
};
var getName = function() {
  alert(4);
};
function getName() {
  alert(5);
}

//请写出以下输出结果：
Foo.getName(); // 2
getName(); // 4
Foo().getName(); // 1
getName(); // 1
new Foo.getName(); // 2
new Foo().getName(); // 3
new new Foo().getName(); // 3
```

> 详细资料可以参考：
[《前端程序员经常忽视的一个 JavaScript 面试题》](https://github.com/Wscats/Good-text-Share/issues/85)
[《一道考察运算符优先级的 JavaScript 面试题》](https://segmentfault.com/q/1010000008430170)
[《一道常被人轻视的前端 JS 面试题》](https://www.cnblogs.com/xxcanghai/p/5189353.html)
------
------

#### 1.2.2.3 [面试题2] 写出下面代码 的输出结果

```js
function a(){
  console.log(c);
}
a();
var c=1;

// undefined; c的定义声明被提升，赋值声明停留在原地。
```



```js
function a(){
  console.log(c);
}
var c=1;
a();
//1 
```



## 1.2 数据类型

### 1.2.1 数据类型的区分

####  1.2.1.1 介绍js的基本数据类型？


+ js 一共有六种基本数据类型，分别是**Undefined、Null、Boolean、Number、String**，还有在 ES6 中新增的 **Symbol**（和 ES10 中新增的 **BigInt** 类型）。
+ **Symbol** 代表创建后独一无二且不可变的数据类型，它的出现我认为主要是为了解决可能出现的全局变量冲突的问题。
+ **BigInt**是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大整数，即使这个数已经超出了 Number 能够表示的安全整数范围。

#### 1.2.1.2  JavaScript 有几种类型的值？你能画一下他们的内存图吗？

> 涉及知识点：

>> 栈：原始数据类型（Undefined、Null、Boolean、Number、String）
>> 堆：引用数据类型（对象、数组和函数）


>  两种类型的区别是：存储位置不同。
   原始数据类型直接存储在栈（stack）中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储。
>  引用数据类型存储在堆（heap）中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。


  回答：

+  js 可以分为两种类型的值，一种是基本数据类型，一种是复杂数据类型。
+ 基本数据类型....（参考1.2.1）
+ 复杂数据类型指的是 Object 类型，所有其他的如 Array、Date 等数据类型都可以理解为 Object 类型的子类。
+ 两种类型间的主要区别是它们的存储位置不同，基本数据类型的值直接保存在栈中，而复杂数据类型的值保存在堆中，通过使用在栈中保存对应的指针来获取堆中的值。

> 详细资料可以参考：[《JavaScript 有几种类型的值？》](https://blog.csdn.net/lxcao/article/details/52749421) [《JavaScript 有几种类型的值？能否画一下它们的内存图；》](https://blog.csdn.net/jiangjuanjaun/article/details/80327342)

####  1.2.1.3  什么是堆？什么是栈？它们之间有什么区别和联系？

  + **堆**和**栈**的概念存在于数据结构中和操作系统内存中。
  + 在数据结构中，栈中数据的存取方式为**先进后出**。而堆是一个**优先队列**，是按优先级来进行排序的，优先级可以按照大小来规定。**完全二叉树**是**堆**的一种实现方式。
  + 在操作系统中，内存被分为栈区和堆区。**栈区内存**由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈。**堆区内存**一般由程序员分配释放，若程序员不释放，程序结束时可能由垃圾回收机制回收。

>  详细资料可以参考：[《什么是堆？什么是栈？他们之间有什么区别和联系？》](https://www.zhihu.com/question/19729973)

####  1.2.1.4 null 和 undefined 的区别？


+ 首先 Undefined 和 Null 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 undefined 和 null。

+ undefined 代表的含义是*未定义*，null 代表的含义是*空对象*。一般变量声明了但还没有定义的时候会返回 undefined，null主要用于赋值给一些可能会返回对象的变量，作为初始化。

+ undefined 在 js 中不是一个保留字，这意味着我们可以使用 undefined 来作为一个变量名，这样的做法是非常危险的，它会影响我们对 undefined 值的判断。但是我们可以通过一些方法获得安全的 undefined 值，比如说 void 0。

+ 当我们对两种类型使用 typeof 进行判断的时候，Null 类型化会返回 “object”，这是一个历史遗留的问题。当我们使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。

> 详细资料可以参考：[《JavaScript 深入理解之 undefined 与 null》](http://cavszhouyou.top/JavaScript%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E4%B9%8Bundefined%E4%B8%8Enull.html)

------

#### 1.2.1.5  Symbol 类型的注意点？

- 1.Symbol 函数前不能使用 new 命令，否则会报错。

- 2.Symbol 函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。

- 3.Symbol 作为属性名，该属性不会出现在 for...in、for...of 循环中，也不会被 Object.keys()、Object.getOwnPropertyNames()、JSON.stringify() 返回。

- 4.Object.getOwnPropertySymbols 方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。

- 5.Symbol.for 接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建并返回一个以该字符串为名称的 Symbol 值。

- 6.Symbol.keyFor 方法返回一个已登记的 Symbol 类型值的 key。


------

### 1.2.2 数据类型的判断

-------

#### 1.2.2.1  typeof NaN 的结果是什么？

+ NaN 意指“不是一个数字”（not a number），NaN 是一个“警戒值”（sentinel value，有特殊用途的常规值），用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。

  ```js
  typeof NaN; // "number"
  ```

+ NaN 是一个特殊值，它和自身不相等，是唯一一个非自反（自反，reflexive，即 x === x 不成立）的值。而 NaN != NaN为 true。

------

#### 1.2.2.2  isNaN 和 Number.isNaN 函数的区别？

+ 函数 **isNaN** 接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的的值都会返回 true，因此非数字值传入也会返回 true ，会影响 NaN 的判断。
+ 函数 **Number.isNaN** 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，这种方法对于 NaN 的判断更为准确。

------

#### 1.2.2.3 typeof 与 instanceof 的区别?

+ **typeof** 操作符(一元运算符）返回一个字符串，表示未经计算的操作数的类型。

运算目标 | 返回值 
:-: | :-: 
Array | "object" 
Boolean | "boolean"  
Function | =="function"== 
Number | "number"
Object | "object"
String | "string"
Undefined  | “undefined” 
Null  | =="object"== 

```js
console.log(typeof 123)   // number
console.log(typeof NaN)  // number
console.log(typeof Infinity) //number
console.log(typeof Number.MIN_VALUE) // number

console.log(typeof true) //boolean
console.log(typeof false) //boolean

console.log(typeof '12rs') 	//string

console.log(typeof undefined) // undefined

console.log(typeof window)  //object
console.log(typeof [])			//object
console.log(typeof null)    	//object

// 函数作为一等公民，可以在typeof判断的时候独立出来。
console.log(typeof Date)   		//function
console.log(typeof  eval)  		//function
console.log(typeof Number) 	// function
```

+ **instanceof** 二元运算符，用于检测构造函数的*prototype*属性是否出现在某个实例对象的*原型链*上。只有引用数据类型（Array，Function，Object，Date ...) 才能被精准判断，其他基础数据类型（Number，Boolean，String) 不能被精确判断。

```js

// 基础数据类型 返回false
console.log(1 instanceof Number)  //false
console.log(true instanceof Boolean) // false
console.log('e' instanceof String) //false

// 引用数据类型 返回true
console.log(new Number(1) instanceof Number) 		 // true
console.log([] instanceof Array)								 //true
console.log([] instanceof Object) 							//true
console.log({} instanceof Object) 							//true
console.log(Date instanceof Function)						// true
console.log(function(){} instanceof Function)		//true
console.log(function(){} instanceof Object) 		//true
```

------

#### 1.2.2.4 如何实现instanceof ？

```js
// instanceof 运算符用于判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。
function myInstanceof(left, right) {
  let proto = Object.getPrototypeOf(left), // 获取对象的原型
    	prototype = right.prototype; // 获取构造函数的 prototype 对象
  // 判断构造函数的 prototype 对象是否在对象的原型链上
  while (true) {
    if (!proto) return false;
    if (proto === prototype) return true;
    // 一直沿着原型链向上获取原型的原型
    proto = Object.getPrototypeOf(proto);
  }
}
```

------

#### 1.2.2.5 valueOf 和 toString 的区别？

+ valueOf()方法返回==对象的原始值==。

  - 该原始方法定义于Object.prototype.valueOf().

  - 默认情况下，valueOf方法由Object后面的每个对象继承。但是**Javascript的许多内置对象都重写了该函数**，以实现适合自身的功能需要。每个内置的核心对象都会覆盖此方法以返回适当的值。


| 对象.valueOf() |              返回值               |
| :------------: | :-------------------------------: |
|     Array      |         **数组**对象本身          |
|    Boolean     |              布尔值               |
|      Date      |              毫秒数               |
|    Function    |             函数本身              |
|     Number     |              数字值               |
|     Object     |             对象本身              |
|     String     |             字符串值              |
|       /        | Math 和 Error 对象没有valueOf方法 |

```js
var colors = ["red", "blue", "green"];
console.log(colors.valueOf()); 
//["red", "blue", "green"]

var bool = true;
console.log(bool.valueOf());  //true

var time = new Date();
console.log(time.valueOf());  //1582165191537

var foo=function(){
    return "function";
};
console.log(foo.valueOf());
//f(){return "function"}

var obj = {
   name: 'js',
   age: 25
};
console.log(obj.valueOf());
//{name:'js',age:25}
```



+ toString()方法返回==表示该对象的字符串==。

	- 该原始方法定义于Object.prototype.toString().默认情况下，toString()方法被每个Object对象继承。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中type是对象的类型。

	- *Array,Boolean,RegExp,String,Number,Function,Date*这些都是覆盖了 Object 的 toString 方法。

```js
var obj = new Object()
obj.toString() // "[object Object]"
```

|    对象    |                            返回值                            |
| :--------: | :----------------------------------------------------------: |
|   Array    | 返回由数组中每个值的字符串形式拼接而成的一个以==逗号分隔的字符串== |
|  Boolean   |                   返回布尔对象的字符串形式                   |
|    Date    |                         毫秒数字符串                         |
|  Function  |                     函数本身源代码字符串                     |
|   Number   |                 返回数字对象的字符串表示形式                 |
| ==Object== |                       [object Object]                        |
|   String   |                          字符串本身                          |
|    null    |                            "null"                            |
| undefined  |                         "undefined"                          |

```js
var colors = ["red","blue","green"];
console.log(colors.toString());
//"red,blue,green"
console.log(colors.toString().split(','));
//["red", "blue", "green"]

//数字数组
var arr = [1, 2, 3, 4];
console.log(arr.toString().split(',').map(item => +item));
//[1, 2, 3, 4]
```
------
#### 1.2.2.6  {} 和 [] 的 valueOf 和 toString 的结果是什么？


+ {} 的 valueOf 结果为 {} ，toString 的结果为 "[object Object]"

+ [] 的 valueOf 结果为 [] ，toString 的结果为 ""

------

#### 1.2.2.7  如何精确判断所有的数据类型？

+ 既然可以使用Object.prototype.toString 来判断 ”继承自Object.prototype 实例对象“的类型(我们可以把它看作一个内部的分类，而非传统的面向对象意义上的类)  。那么可以**使用Object.prototype.toString.call(\*)来精确判断所有的数据类型**。

```js
 var toString = Object.prototype.toString;
 toString.call(233); 					//[object Number]
 toString.call('aaa'); 				//[object String]
 toString.call(true); 				//[object Boolean]
 toString.call([]); 					// [object Array]
 toString.call(function(){}); // [object Function]
 toString.call({}); 					// [object Object]

 toString.call(Math);					//[object Math]
 toString.call(new Date());   // [object Date]
 toString.call(new String()); // [object String]
 toString.call(undefined);    // [object undefined]
 toString.call(null); 				// [object Null]	
```

--------
#### 1.2.2.8 如何封装一个 javascript 的类型判断函数？
```js
function getType(value) {
  // 判断数据是 null 的情况
  if (value === null) {
    return value + "";
  }
  // 判断数据是引用类型的情况
  if (typeof value === "object") {
    let valueClass = Object.prototype.toString.call(value),
        type = valueClass.split(" ")[1].split("");
		type.pop();
		return type.join("").toLowerCase();
  } else {
    // 判断数据是基本数据类型的情况和函数的情况
    return typeof value;
  }
}
```
------
#### 1.2.2.9  内部属性 [[Class]] 是什么？


+ 所有 typeof 返回值为 "object" 的对象（如数组）都包含一个内部属性 [[Class]]，我们可以把它看作一个（引用类型Object）内部的分类，而非传统的面向对象意义上的类。这个属性无法直接访问，一般通过 Object.prototype.toString(..) 来查看。例如：

```js
Object.prototype.toString.call( [1,2,3] );
// "[object Array]"

Object.prototype.toString.call( /regex-literal/i );
// "[object RegExp]"

// 我们自己创建的类就不会有这份特殊待遇，因为 toString() 找不到 toStringTag 属性时只好返回默认的 Object 标签
// 默认情况类的[[Class]]返回[object Object]
class Class1 {}
Object.prototype.toString.call(new Class1()); // "[object Object]"
// 需要定制[[Class]]
class Class2 {
  get [Symbol.toStringTag]() {
    return "Class2";
  }
}
Object.prototype.toString.call(new Class2()); // "[object Class2]"
```

------
#### 1.2.2.10 如何判断一个对象是否为空对象？
```js
function checkNullObj(obj) {
  return Object.keys(obj).length === 0;
}
```
> 详细资料可以参考：
[《js 判断一个 object 对象是否为空》](https://blog.csdn.net/FungLeo/article/details/78113661)
------



### 1.2.3 数据类型的比较与转换

------

#### 1.2.3.1  其他值到字符串的转换规则？


+ 规范的 9.8 节中定义了抽象操作 ToString ，它负责处理非字符串到字符串的强制类型转换。

	- （1）Null 和 Undefined 类型 ，null 转换为 "null"，undefined 转换为 "undefined"，

	- （2）Boolean 类型，true 转换为 "true"，false 转换为 "false"。

	- （3）Number 类型的值直接转换，不过那些极小和极大的数字会使用指数形式。

	- （4）Symbol 类型的值直接转换，但是只允许显式强制类型转换，使用隐式强制类型转换会产生错误。

	- （5）对普通对象来说，除非自行定义 toString() 方法，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值。
	
------

#### 1.2.3.2  其他值到数字值的转换规则？


+ 有时我们需要将非数字值当作数字来使用，比如数学运算。为此 ES5 规范在 9.3 节定义了抽象操作 ToNumber。

	- （1）Undefined 类型的值转换为 NaN。

	- （2）Null 类型的值转换为 0。

	- （3）Boolean 类型的值，true 转换为 1，false 转换为 0。

	- （4）String 类型的值转换如同使用 Number() 函数进行转换，如果包含非数字值则转换为 NaN，空字符串为 0。

	- （5）Symbol 类型的值不能转换为数字，会报错。

	- （6）对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

+ 	为了将值转换为相应的基本类型值，抽象操作 ToPrimitive 会首先（通过内部操作 DefaultValue）检查该值是否有valueOf() 方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString() 的返回值（如果存在）来进行强制类型转换。如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。

------

#### 1.2.3.3  其他值到布尔类型的值的转换规则？

+ ES5 规范 9.2 节中定义了抽象操作 ToBoolean，列举了布尔强制类型转换所有可能出现的结果。

	- 以下这些是假值：undefined、null、false、+0、-0、NaN 和 ""
	- 假值的布尔强制类型转换结果为 false。从逻辑上说，假值列表以外的都应该是真值。

------


#### 1.2.3.4  什么情况下会发生布尔值的隐式强制类型转换？


+ （1） if (..) 语句中的条件判断表达式。
+ （2） for ( .. ; .. ; .. ) 语句中的条件判断表达式（第二个）。
+ （3） while (..) 和 do..while(..) 循环中的条件判断表达式。
+ （4） ? : 中的条件判断表达式。
+ （5） 逻辑运算符 ||（逻辑或）和 &&（逻辑与）左边的操作数（作为条件判断表达式）。

------

#### 1.2.3.5  || 和 && 操作符的返回值？


+ || 和 && 首先会对第一个操作数执行条件判断，如果其不是布尔值就先进行 ToBoolean 强制类型转换，然后再执行条件判断。

+ 对于 || 来说，如果条件判断结果为 true 就返回第一个操作数的值，如果为 false 就返回第二个操作数的值。

+ && 则相反，如果条件判断结果为 true 就返回第二个操作数的值，如果为 false 就返回第一个操作数的值。

+ || 和 && 返回它们其中一个操作数的值，而非条件判断的结果

------

#### 1.2.3.6  Symbol 值的强制类型转换？

+ ES6 允许从符号到字符串的显式强制类型转换，然而隐式强制类型转换会产生错误。
+ Symbol 值不能够被强制类型转换为数字（显式和隐式都会产生错误），但可以被强制类型转换为布尔值（显式和隐式结果都是 true ）。

------
#### 1.2.3.7   == 操作符的强制类型转换规则？

（1）字符串和数字之间的相等比较，将字符串转换为数字之后再进行比较。

（2）其他类型和布尔类型之间的相等比较，先将布尔值转换为数字后，再应用其他规则进行比较。

（3）null 和 undefined 之间的相等比较，结果为真。其他值和它们进行比较都返回假值。

（4）对象和非对象之间的相等比较，对象先调用 ToPrimitive 抽象操作后，再进行比较。

（5）如果一个操作值为 NaN ，则相等比较返回 false（ NaN 本身也不等于 NaN ）。

（6）如果两个操作值都是对象，则比较它们是不是指向同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回 true，否则，返回 false。


> 详细资料可以参考：[《JavaScript 字符串间的比较》](https://www.jeffjade.com/2015/08/28/2015-09-02-js-string-compare/)
------

#### 1.2.3.8 如何将字符串转化为数字，例如 '12.3b'?

（1）使用 Number() 方法，前提是所包含的字符串不包含不合法字符。

（2）使用 parseInt() 方法，parseInt() 函数可解析一个字符串，并返回一个整数。还可以设置要解析的数字的基数。当基数的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。

（3）使用 parseFloat() 方法，该函数解析一个字符串参数并返回一个浮点数。

（4）使用 + 操作符的隐式转换。

> 详细资料可以参考：[《详解 JS 中 Number()、parseInt() 和 parseFloat() 的区别》](https://blog.csdn.net/m0_38099607/article/details/72638678)

------

#### 1.2.3.9 如何将浮点数点左边的数每三位添加一个逗号，如 12000000.11 转化为『12,000,000.11』?

```js
// 方法一
function format(number) {
  return number && number.replace(/(?!^)(?=(\d{3})+\.)/g, ",");
}
// 方法二
function format1(number) {
  return Intl.NumberFormat().format(number)
}
// 方法三 ✅
function format2(number) {
  return number.toLocaleString('en')
}
```

------

#### 1.2.3.10  Object.is() 与原来的比较操作符 “ === ” 、 “ == ” 的区别？

+ 使用双等号进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。

+ 使用三等号进行相等判断时，如果两边的类型不一致时，不会做强制类型准换，直接返回 false。

+ 使用 **Object.is(value1,value2)**来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如 -0 和 +0 不再相等，两个 NaN 认定为是相等的。

------

------



## 1.3 函数、作用域与内存
------
### 1.3.1 js的词法作用域

+ 作用域是指程序源代码中定义变量的区域。
+ 作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。
+ JavaScript 采用==词法作用域==(lexical scoping)，也就是静态作用域。

> 详细资料可以参考：[《JavaScript深入之词法作用域和动态作用域》](https://github.com/mqyqingfeng/Blog/issues/3)
------
### 1.3.2 执行上下文栈

+ 变量或函数的上下文决定了他们可以访问哪些数据，以及他们的行为。执行上下文，即”代码的执行环境“。
+ js可执行代码的类型有哪些？（执行上下文的类型？代码的执行环境？）
	- 全局代码，函数代码，eval代码。
+ 执行上下文有三个重要属性：变量对象，作用域链，this。

> 详细资料可以参考：[《JavaScript深入之执行上下文栈》](https://github.com/mqyqingfeng/Blog/issues/4) 、 [《JavaScript深入之执行上下文》](https://github.com/mqyqingfeng/Blog/issues/8) 

------
### 1.3.3 变量对象

+ 变量对象（variable object，VO）
+ 变量对象是与执行上下文相关的数据作用域，存储了在上下文中定义的变量和函数声明。

> 详细资料可以参考：[《JavaScript深入之变量对象》](https://github.com/mqyqingfeng/Blog/issues/5)

------

### 1.3.4 作用域链

+ 当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。
+ 作用域链的本质上是一个**指向变量对象指针列表**。变量对象是一个包含了执行环境中所有变量和函数的对象。作用域链的前端始终都是当前执行上下文的变量对象。全局执行上下文的变量对象（也就是全局对象）始终是作用域链的最后一个对象。当我们查找一个变量时，如果当前执行环境中没有找到，我们可以沿着作用域链向后查找。
+ 作用域链的作用是保证对执行环境有权访问的所有变量和函数的有序访问，通过作用域链，我们可以访问到外层环境的变量和函数。
+ 作用域链的创建过程跟执行上下文的建立有关。

> 详细资料可以参考：[《JavaScript 深入理解之作用域链》](https://github.com/mqyqingfeng/Blog/issues/6)

------

### 1.3.5 this 的指向？


+ this 提供了一种更优雅的方式来”隐式传递”一个对象引用，因此可以将API设计得更加简洁并且易于复用。在js中，this的存在，使得函数可以自动引用合适的上下文对象。

+ this 是在运行时进行绑定的。this的绑定和函数的声明位置没有任何关系，只取决于函数的调用方式。

+ setTimeout()函数里面，`function`关键字命名的匿名函数里的this会指向window（非严格模式）。

+ this 是执行上下文中的一个属性，它指向最后一次**调用**这个方法的对象。在实际开发中，this 的指向可以通过下面的顺序（四种调用模式）来判断。
	- (1) 构造器调用，函数是否在new中调用（new绑定）。如果一个函数用 new 调用时，函数执行前会新创建一个对象，this 指向这个新创建的对象。
	```js
	var bar = new foo()
	```
	- (2) 函数是否通过call、apply、bind（显示绑定）调用。如果是，this绑定的是指定的对象。
	```js
	var bar = foo.call(obj1);
	```
	- (3)  函数是否在某个上下文对象中调用（隐式绑定）？如果是，this绑定的是那个上下文对象。
	```js
	var bar = obj2.foo()
	```
	- (4)  函数内部直接调用（默认绑定），严格模式绑定到undefined，非严格模式绑定到全局对象。
	```js
	var bar = foo()
	```

> 关于this的详解，参考：[《JavaScript 深入理解之 this 详解》](https://github.com/mqyqingfeng/Blog/issues/7)

#### 1.3.5.1 关于this的面试题1：判断下面函数的输出。

```js
// 提示：注意判断执行上下文的是如何进出栈的。
var length = 1000;
var obj = {
  length:10,
  fn:function(fn){
    fn();      
    arguments[0]();
  }
};
function fn(){
  console.log(this.length);
}
obj.fn(fn,1,2,3)

/**答案
	1000
	4
**/
// 解释：首先输出1000，是因为第一个函数是直接调用的，其内部this指向window。其次输出4 . 是因为fn是用过arguments对象调用的，其内部this指向arguments对象。这里的arguments对象关联的函数是obj.fn。obj.fn()在调用的时候才能具体确定arguments对象的值。
```



------

### 1.3.6 显式绑定this的几种内置函数（call，apply，bind）
> ECAMScript 3给Function的原型定义了两个方法,它们是Function.prototype.call和Function.prototype.apply 。
>
> 当使用 call 或者 apply 的时候,如果我们传入的第一个参数为 null,函数体内的 this 会指 向默认的宿主对象,在浏览器中则是 window 。但如果是在严格模式下,函数体内的 this 还是为 null.

+ **apply** 接受两个参数,第一个参数<u>指定了函数体内 this 对象的指向</u>，第二个参数为一个带下标的集合,这个集合可以为**数组**,也可以为**类数组** , apply 方法把这个集合中的元素作为参数传递给被调用的函数

```js
var func = function(a,b,c){
	console.log([a,b,c]);// [1,2,3]
};
func.apply(null,[1,2,3])
```
+ **call** 传入的参数数量不固定，跟 apply 相同的是，第一个参数也是<u>代表函数体内的 this 指向</u>，从第二个参数开始往后，每个参数被依次传入函数。

```js
var func = function( a, b, c ){ 
   console.log([a,b,c]); //输出:[1,2,3]
};
func.call( null, 1, 2, 3 );
```
+ call 和 apply 可以给函数传参，并控制函数调用的上下文（改变this的指向），这样可以用来借用其他对象的方法。
```js
var a=Math.max.apply( null, [ 1, 2, 5, 3, 4 ] );
console.log(a);// 输出:5

function sum(num1, num2) {
    return num1 + num2;
};
function callSum(num1, num2) {
    return sum.call(this, num1,num2)
};
console.log(callSum(1, 2))
```
+ ES5 给定了新方法：**bind()**。 bind() 方法会创建一个新的**函数**，其this值会被绑定到传给bind()的对象。**bind() **方法创建一个新的函数，在 `bind()` 被调用时，这个新函数的 `this` 被指定为 `bind()` 的第一个参数，而其余参数将作为新函数的参数，供调用时使用

```js
window.color = 'red';
var o = {
	color:'blue';
};
function sayColor(){
	console.log(this.color);
};
let objSayColor = sayColor.bind(o);
objSayColor();// blue
```

------

#### 1.3.6.1 手写call函数

```js
// 实现 abc.myCall(A,1,2,3) ,  第二个参数以后是个参数列表。

Function.prototype.myCall = function(context){
  // 【1】判断调用myCall这个函数的对象（函数abc）（这个对象[函数abc]被this指向了：this的隐式调用）：
  if(typeof this !== 'function'){
    throw new TypeError('Error');
    return;
  };
  let result = null;
  // 【2】获取参数(abc需要的参数）： 第一个参数为this绑定的对象，从第二个参数开始截取
  let args = [...arguments].slice(1);
  // 【3】 判断上下文对象是否存在,如果不存在设置为window
  context = context || window;
  // 【4】将函数（abc）[this 指向他] 作为上下文对象（A)的一个属性
  context.fn = this;
  // 【5】 使用上下文对象来调用这个方法，并保存返回结果。
  result = context.fn(...args);
  // 【6】 删除给上下文对象添加的属性
  delete context.fn;
  // 【7】 返回结果
  return result;
}
```

------

#### 1.3.6.2 手写apply函数

```js
// 实现 abc.myApply(A,[1,2,3]) , 第二个参数是数组（或类数组）。

Function.prototype.myApply = function(context){
  // 【1】判断调用myCall这个函数的对象（函数abc）（这个对象[函数abc]被this指向了：this的隐式调用）：
  if(typeof this !== 'function'){
    throw new TypeError('Error');
    return;
  };
  let result = null;
  // 【2】判断 context 是否存在，如果未传入则为 window
  context = context || window ; 
  // 【3】获取第二参数(abc需要的参数）
  let args = arguments[1];
  // 【4】将函数（abc）[this 指向他] 作为上下文对象（A)的一个属性
  context.fn = this;
  // 【5】 第二参数必须存在且是数组类型
  if(args && Array.isArray(args)){
     result = context.fn(...args);
     delete context.fn;
  }else{
     delete context.fn;
     throw new Error('Error');
     return;
  };
  return result;
}
```

#### 1.3.6.3 手写bind函数

```js
// 实现 abc.bind(A,...),在 bind() 被调用时，这个新函数的 this 被指定为 bind() 的第一个参数，而其余参数将作为新函数的参数，供调用时使用.
// 同时要兼顾【柯里化传参】

 Function.prototype.myBind = function () {
   if (typeof this !== 'function') {
      throw new TypeError('Error')
      return
   };
   let args = Array.prototype.slice.call(arguments);
      // 绑定的对象是第一个参数
   const context = args.shift();
   const self = this;
   return function Fn() {
      let args2 = Array.prototype.slice.call(arguments);
      return self.apply( context,args.concat(args2) )
   };
}
```

> 举例验证：兼顾柯里化传参（柯里化参考1.6.4）
```js
function fn1(a, b, c) {
   console.log('this.x', this.x);
   console.log(a, b, c)
}
const fn2 = fn1.myBind({ x: 1 }, 2, 4) // 第二、三个参数：2,4 分别传给了a、b
//const res = fn2(5); // 这里的参数5 传给了c
// this.x 1
// 2 4 5
const obj = new fn2(7)
```
------
> 上面的代码并没有兼顾fn2 函数用作构造函数，通过new调用生成实例对象：this 与绑定对象无关，实例继承调用函数原型上的属性。
> **更优方案**如下:
+ ✅最佳实践：
```js
Function.prototype.myBind = function () {
   if (typeof this !== 'function') {
        throw new TypeError('Error')
        return
    };
   let args = Array.prototype.slice.call(arguments);
   const context = args.shift();
   const self = this;
   let o = function () {};
  // 以下返回的Fn函数是我们要关注的myBind实现的对abc函数的改造结果。
   let Fn = function () {
      let args2 = Array.prototype.slice.call(arguments);
        // 【6】判断Fn是通过new调用的构造函数还是被直接调用的函数
      return self.apply(
          this instanceof o ? this : context,
          args.concat(args2)
        )
    };
   o.prototype = self.prototype;
      // 利用o函数实现中继，=> 原型式继承
   Fn.prototype = new o;
   return Fn;
}
```
> 举例验证 : fn1.bind()返回的fn2实现new fn2 应该实现的功能：
>
> （1）new fn2生成的实例，要继承fn1构造函数原型上的属性。
>
> （2）new 构造函数，其内的this指向生成的实例对象。
```js
function fn1(a, b, c) {
   console.log('this.x', this.x);
   console.log(a, b, c);
  return 'Hello'
}
fn1.prototype.d = '定义在fn1上的d属性';
const fn2 = fn1.myBind({ x: 1 }, 2, 4);
const obj1 = new fn2('thirdP')  // 'this.x'  undefined (new规则下，this和绑定对象无关) 2 4 'thirdP'
const obj2 = fn2('thirdP');     // 'this.x' 1 (直接调用，this绑定到指定的对象) 2 4 thirdP
console.log(obj1.d) //"定义在fn1上的d属性"
console.log(obj2) // "Hello"
```
------

+ 总结：为了实现bind函数要考虑的事项？
	- (1) 通过调用fn1.bind(A,...)返回的函数实例fn2，无论是`new fn2()`调用还是`fn2()`直接调用，都不要忽视`柯里化传参`。
	- (2) 如果fn2通过`new fn2()`调用，fn2作为一个构造函数，调用时其内部的this指向new调用返回的实例对象。同时，这个实例对象可以从fn1函数的原型对象上继承属性。
	- (3) 如果通过`fn2()`直接调用，那么调用时其内部的this指向其传入的第一个参数，这也是字面量意义上的绑定对象；此外，如果fn1内有返回值，还要返回fn1函数的返回值。

------

### 1.3.7 什么是闭包？为什么要使用闭包？

+ **闭包**是指引用了另一个函数作用域中变量的函数，创建闭包的最常见的方式就是在一个函数内创建另一个函数，创建的函数可以访问到当前函数的局部变量。

+ 闭包有两个常用的用途：

  - 闭包的第一个用途是使我们在函数外部能够访问到函数内部的变量。通过使用闭包，我们可以通过在外部调用闭包函数，从而在外部访问到函数内部的变量，可以使用这种方法来创建私有变量。
    
- 函数的另一个用途是使已经运行结束的函数上下文中的变量对象继续留在内存中，因为闭包函数保留了这个变量对象的引用，所以这个变量对象不会被回收。
  
```js
var scope = "global scope";
function checkscope(){
	var scope = "local scope";
	function f(){
		return scope;
	};
	return f;
}
var foo=checkscope();
foo();
// 注意关键：f函数执行的时候，checkscope函数上下文已经从执行上下文栈中弹出，但是因为闭包，所以f执行上下文维护了一个作用域链：即使 checkscopeContext 被销毁了，但是 JavaScript 依然会让 checkscopeContext.AO 活在内存中，f 函数依然可以通过 f 函数的作用域链找到它，正是因为 JavaScript 做到了这一点，从而实现了闭包这个概念。
```

+ 其实`闭包`的本质就是作用域链的一个特殊的应用，只要了解了作用域链的创建过程，就能够理解闭包的实现原理。

> 详细资料可以参考：[《JavaScript 深入理解之闭包》](https://github.com/mqyqingfeng/Blog/issues/9) 、[《技术蛋讲解闭包bilibili》](https://www.bilibili.com/video/BV1iE411q7Qd)

#### 1.3.7.1 修改下面代码，使其循环输出结果依次为1，2，3，4，5.

```js
for(var i=1;i<=5;i++){
	setTimeout(function timer(){
		console.log(i);
	},i*1000);
};

// 方法1：计时函数外层包裹立即执行函数，提供闭包条件
for(var i=1;i<=5;i++){
  (function(i){
    setTimeout(function timer(){
      console.log(i);
    },i*1000)
  })(i)
};

// 方法2：计时器函数的【第一个参数】使用立即执行函数，提供闭包条件
for(var i=1;i<=5;i++){
  setTimeout((function(i){
    return function(){
      console.log(i);
    }
  })(i),i*1000);
};

// 方法3：es6，let声明
for(let i=1;i<=5;i++){
  setTimeout(function timer(){
    console.log(i);
  },i*1000);
};
```
#### 1.3.7.2 下面函数输出了什么？
```js
for (var i = 0; i < 5; i++) {
   setTimeout(function () {
      console.log(i++);
   }, 4000)
}
// (4s以后)
// 5
// 6
// 7
// 8
// 9
```
#### 1.3.7.3 下面函数输出了什么？
```js
for (var i = 0; i < 5; i++) {
   setTimeout(function () {
      console.log(i++);
   }, 4000)
}
console.log(i)
// 输出：
//5   (立即输出)

//5  （4秒以后）
//6
//7
//8
//9

// 这里涉及同步任务与异步任务。
// 同步任务i循环五次后，加1，最后一行打印出5. 这期间，任务队列中有五个匿名函数等待执行。
// 4秒之后，队列任务依次执行，首先打印i，i=5，之后再给i+1，一共执行五次。
```
------
#### 1.3.7.4 下面函数输出了什么？
```js
for(var i=0;i<5;i++){
  setTimeout(function(){
    console.log(i);
  },1000*i)
};
// 立即输出5，然后每隔1s输出一个5。最后一共输出5个5.
// 任务队列类里面一次是 0s后cosole.log(i) ; 1s后console.log(i); 2s后console.log(i)。。。
```

### 1.3.8  js中的所有参数都是按值传递的。

+ 参考《高程4》P85页。

+ 参考[《JavaScript 深入理解之参数按值传递》](https://github.com/mqyqingfeng/Blog/issues/10)
+ 高赞评论。

```
我个人认为你的理解有误, 红宝书说ECMAScript中所有函数的参数都是按值传递的, 这是没错的. 关键在于如何理解值传递和引用类型, 这个概念我很早在C#上深入研究一番(在<C#本质论>的指导下). 而JavaScript的引擎是C++实现的, 所以在这一块概念上C#与C++大致一样.
C#的数据类型分为2种: 值类型和引用类型, 而方法参数的传递方式也分为2种: 值传递和引用传递, 这里要强调的是数据类型和方法参数的传递方式没有半毛钱关系. 这两者排列组合后得到4种情况: 1. 方法参数类型是值类型, 用值传递; 2. 方法参数类型是引用类型, 用值传递; 3. 方法参数类型是值类型, 用引用传递; 4. 方法参数类型是引用类型, 用引用传递.
ECMAScript如何实现方法参数用引用传递, 我实际使用中没用到过, 这里不敢妄言, 但是你在"引用传递"中举的例子, 很明显是错误的, 它只是方法参数是引用类型, 但是用的是值传递方式, 这也印证了红宝书上说的那句话.
下面我先说说C#里的这4种情况.
首先, 弄清楚方法参数传递方式. C#区分值传递和引用传递很方便, 方法参数前加ref(out修饰符这里不讨论)就是引用传递, 什么都不加就是值传递. 我们都知道方法参数有实参和形参之说, 而参数传递方式说的就是从实参给形参复制的过程. 值传递就是把实参在内存栈中的数据传递给形参, 然后你在方法内部就可以使用形参了, 而引用传递是把实参的内存栈的地址编号传递给形参.
其次, 弄清楚数据类型, 值类型就是内存中某个地址直接保存了值, 比如int i = 10;(js对应写法: var i = 10;), 运行时会在内存的栈中分配一个地址001, 并在这个地方保存10. 而引用类型则需要在内存中某个地址先保存实际的对象实例, 然后在内存的另一个地址保存指向那个对象实例的指针, 比如MyClass obj = new MyClass { value = 10 };(js对应写法: var obj = { value: 10 };), 运行时首先在内存的托管堆中保存一个MyClass的实例对象, 它的属性value=10, 再到内存的栈中分配一个地址002, 并在这里保存在托管堆中那个对象的内存地址(我们可以把这个内存地址简化理解成指向对象实例的指针). 这就是值类型和引用类型的区别.
回过来再看你的例子, 第一个是"按值传递", 这个例子符合方法参数是值类型并用值传递这种情况, value是值类型, 它在内存栈中的地址001保存了1这个数值, 在foo(value);这句, value是实参, 而foo函数声明中的v是形参, js引擎在内存栈中为形参v分配了一个地址002, 其中也保存了1这个值, 这时修改v的值, 是修改内存地址002里的值, 而地址001里的值没变, 所以在foo函数执行完, 再打印value时, 依然是1.
接下来看第二个"引用传递", 我认为这个说法是错误的, 正确的说法应该是引用类型并用值传递. obj是引用类型, 它需要在内存堆中(js引擎可能不存在托管的概念, 所以这里称为内存堆)分配一个内存地址012, 保存了它的一个对象(属性value和其值1, 这句说的不严谨, 不过不影响对本例的分析), 并在内存栈中分配了一个地址011, 这个地址保存了012(就是那个内存堆的地址, 可以理解为指针). 在foo(obj);这句, obj是实参, 而foo函数声明中的o是形参, js引擎在内存栈中为形参o分配了一个地址013, 其中也保存了012这个值, 012其实并不是像前一个例子中说的1那样的数值, 而是一个内存地址, 所以如果你打印o这个形参, 它不会把012这个值打印出来, 而是把012内存地址里保存的实例对象给打印出来. 到这里就很清楚了, 如果你修改了012指向的那个对象的属性value的值, 那么当你在打印obj这个实参时, 它的obj.value会打印出2, 而不是1.
你的第三个例子"共享传递", "共享传递"这个概念我不是很清楚, 但我觉得你举的这个例子依然是值传递, 唯一与C#不同的是, C#的变量类型定义后不能改变, 而JS的变量类型是可以随意改变的, 因此这个例子无法跟C#中的值传递来类比. 再来分析你这个例子, 首先obj实例化一个对象, 有一个属性value, 值为1, 在内存中就是现在内存堆中分配一个内存空间, 其地址为022, 保存了一个对象(包括它的属性value和值1), 然后再到内存栈中分配一个内存地址021, 保存了内存地址022这个值. 在foo(obj);这句, obj是实参, 而o是形参, 这时在内存栈中给形参o分配了一个地址023, 也保存022这个值(如果在o=2;之前打印o, 将输出undefined, 这里是由于在foo函数作用域内对变量o进行赋值操作, 因此在这个作用域内使用了局部变量o覆盖了形参o, 而局部变量o在使用时没有声明, 所以js引擎会把它的声明提升到作用域最顶部, 因此在赋值语句之前打印, 会输出undefined, 声明提升这个概念暂时也不深入展开感谢@daizengyu123 的指正, 这里因为调用foo函数时给形参o赋值了, 所以在调用o = 2;之前打印, 会输出对象{value: 1}), 而在foo函数中, 又给形参o重新赋值2, 由于2是Number类型, 这是值类型, 因此不用在内存堆中存储数据, 直接在内存栈中即可, 这句赋值语句, 相当于把内存地址023中的值022改为2, 而并没有修改内存地址021(也就是变量obj)的值, 所以在调用foo函数之后再打印obj.value时, 仍然打印出1. 这里如果把o = 2;这句替换为o = { value = 5, other = "abc" };也是同理.
最后补充一下C#中的引用类型的值传递和引用类型的引用传递的对比. 简单来说, 引用类型的值传递, 在方法内部如果对形参重新赋值, 哪怕是同一个类的对象, 在赋值后修改对象的属性, 实参的对应的属性值都不会改变, 同时实参指向的对象也不变, 而形参在重新赋值后已经指向一个新的对象了; 而引用类型的引用传递, 在方法内部如果对形参重新赋值, 那么实参也跟着重新赋值, 实参最初所指向的那个对象将不被任何变量所指向.
```

------

### 1.3.9  类数组对象

#### 1.3.9.1 什么是类数组对象？

+ 一个拥有 length 属性和若干索引属性的对象就可以被称为**类数组对象**，类数组对象和数组类似，但是不能调用数组的方法。

+ 常见的类数组对象有 arguments 和 DOM 方法的返回结果，还有一个函数也可以被看作是类数组对象，因为它含有 length属性值，代表可接收的参数个数。

#### 1.3.9.2 常见的 类数组转换为数组的方法有哪些？

+ （1）通过 call 调用数组的 slice 方法来实现转换

  ```js
  Array.prototype.slice.call(arrayLike);
  ```

+ （2）通过 call 调用数组的 splice 方法来实现转换

  ```js
  Array.prototype.splice.call(arrayLike, 0);
  ```

+ （3）通过 apply 调用数组的 concat 方法来实现转换

  ```js
  Array.prototype.concat.apply([], arrayLike);
  ```

+ （4）通过 Array.from 方法来实现转换

  ```js
  Array.from(arrayLike);
  ```

+  （5）通过[...arrayLike] 转化为数组
```js
  // 某函数内部
  var arr = [...arguments];
```
> 详细的资料可以参考：[《JavaScript 深入之类数组对象与 arguments》](https://github.com/mqyqingfeng/Blog/issues/14)、[《javascript 类数组》](https://segmentfault.com/a/1190000000415572)、[《深入理解 JavaScript 类数组》](https://blog.lxxyx.cn/2016/05/07/%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3JavaScript%E7%B1%BB%E6%95%B0%E7%BB%84/)

------

### 1.3.10 垃圾回收

> Javascript 是使用垃圾回收的语言，也就是说执行环境负责在代码执行时管理内存。通过自动内存管理实现内存分配和闲置资源回收。垃圾回收程序必须追踪和记录哪个变量还会使用，哪个变量不再使用了，以便回收内存。
------
#### 1.3.10.1 垃圾回收的两种策略

+ **标记清理**：当变量进入上下文，这个变量会被加上存在于上下文中的`标记`。当变量离开上下文时，也会被加上离开上下文的`标记`。垃圾回收程序运行时，会标记内存中存储的所有变量，然后他会将所有在上下文中的变量，以及被在上下文中的变量引用的变量的`标记`去掉。在此之后再被加上`标记`的变量就是待删除的了，原因是任何在上下文中的变量都访问不到它们了。随后垃圾回收程序会做一次`内存清理`，销毁带标记的所有值并回收它们的内存。
+ **引用计数**：统计对象被引用的次数，每次发现对象被引用了，就给它次数+1，删除引用就-1，为0的时候就可以回收。引用计数这种策略使用较少，同时可能会出现循环引用的问题。JS引擎不再使用这种算法，因为js会访问非原生js对象，如dom元素。

------

#### 1.3.10.2 垃圾回收与内存管理

+ ***通过let和const 声明提升性能***：这两个ES6新增的关键字有助于改善垃圾回收的过程，因为两者都以块为作用域，相比使用var，使用这两个关键字可能会更早地让垃圾回收程序接入，尽早地回收应该回收的内存。

+ V8引擎将解释后的js代码编译为实际的机器码时，会利用”隐藏类“，使用delete关键字会导致生成相同的隐藏类片段。隐藏类的改变频率和大小可能会对性能产生明显影响。最佳实践是***把不想要的属性设置为null***。这样可以保持隐藏类不变和继续共享，同时也能达到删除引用值供垃圾回收程序回收的效果。

------

#### 1.3.10.3 哪些操作会造成内存泄漏？

- 1.意外的全局变量
- 2.被遗忘的计时器或回调函数
- 3.脱离 DOM 的引用
- 4.闭包
> (1) 第一种情况是我们由于使用未声明的变量，而意外的创建了一个全局变量，而使这个变量一直留在内存中无法被回收。
> (2) 第二种情况是我们设置了 setInterval 定时器，而忘记取消它，如果循环函数有对外部变量的引用的话，那么这个变量会被一直留在内存中，而无法被回收。
> (3) 第三种情况是我们获取一个 DOM 元素的引用，而后面这个元素被删除，由于我们一直保留了对这个元素的引用，所以它也无法被回收。
> (4)第四种情况是不合理的使用闭包，从而导致某些变量一直被留在内存当中。

> 详细资料可以参考：[《JavaScript 内存泄漏教程》](http://www.ruanyifeng.com/blog/2017/04/memory-leak.html)、[《4 类 JavaScript 内存泄漏及如何避免》](https://jinlong.github.io/2016/05/01/4-Types-of-Memory-Leaks-in-JavaScript-and-How-to-Get-Rid-Of-Them/)、[《杜绝 js 中四种内存泄漏类型的发生》](https://juejin.im/entry/5a64366c6fb9a01c9332c706)、[《javascript 典型内存泄漏及 chrome 的排查方法》](https://segmentfault.com/a/1190000008901861)

------

## 1.4 面向对象编程

### 1.4.1 创建对象的多重方式及优缺点?

+ 1. **工厂模式**：工厂模式的主要原理是用函数来封装创建对象的细节，从而通过调用函数来达到复用的目的。但是他有一个很大的问题就是穿件出来的对象无法和某个类型联系起来，它只是简单的封装了复用代码，而*没有建立起对象和函数类型间的关系*。

```js
function createPerson(name,age,job){
	let o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function(){
		console.log(this.name);
	};
	return o;
}
let person1 = createPerson('Nicholas',29,'Software Engineer');
let person2 = createPerson('Greg','27,'Doctor');
```

+ 2. **构造函数模式**：
	- （1）使用new操作符，调用构造函数创建对象，具体发生了什么？
		+ ① 在内存中创建一个新对象；
		+ ② 这个新对象内部的__proto__特性被赋值为构造函数的prototype属性；
		+ ③ 构造函数内部的this指向这个新创建的对象；
		+ ④ 执行构造函数内部的代码；
		+ ⑤ 如果构造函数返回非空对象，则返回该对象；否则返回刚创建的新对象。
	+ （2）构造函数有什么优缺点？
		- ① 构造函数模式相对于工厂模式的优点是，所创建的对象和构造函数建立起了联系，因此我们可以通过原型来识别对象的类型。
		- ② 构造函数存在的一个缺点是，其*定义的方法会在每个实例上都创建一遍，浪费了不必要的内存空间*。

```js
function Person(name,age,job){
	this.name = name;
	this.age =age;
	this.job = job;
	this.sayName = function(){ // 这里逻辑等价于 new Function(...)
		console.log(this.name);
	};
}
let person1 = createPerson('Nicholas',29,'Software Engineer');
let person2 = createPerson('Greg','27,'Doctor');
person1.sayName();// Nicholas
person2.sayName();// Greg
```

+ 3. **原型模式**：
	- 因为每一个函数都有一个 prototype 属性，这个属性是一个对象，它包含了通过构造函数创建的所有实例都能共享的属性和方法。因此我们可以使用原型对象来添加公用属性和方法，从而实现代码的复用。这种方式相对于构造函数模式来说，解决了函数对象的复用问题。但是这种模式也存在一些问题，一个是没有办法通过传入参数来初始化值，另一个是如果存在一个引用类型如 Array 这样的值，那么所有的实例将共享一个对象，*一个实例对引用类型值的改变会影响所有的实例*。

```js
function Person(){}
Person.prototype = {
	constructor:Person,
	name:"Nicholas",
	age:29,
	job:'software Engineer',
	friends:["Shelby","Court"] // 属性值为“引用类型”
}
let person1 = new Person();
let person2 = new Person();
person1.friends.push("Van"); 
console.log(person2.friends); // "Shelby,Court,Van"
```
> 一般来说，不同的实例应该有属于自己的属性副本，这就是实际开发中通常不使用原型模式的原因。

> 详细资料可以参考：[《JavaScript 深入理解之对象创建》](http://cavszhouyou.top/JavaScript%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E4%B9%8B%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA.html)

------
### 1.4.2 对象的比较

#### 1.4.2.1 如何判断两个数组相等(键值对完全相等）？
> 涉及到简单类型数据的比较和复杂数据类型的比较。复杂数据类型可以通过递归化解为简单数据类型的比较。
```js
// 用来过滤掉简单的类型比较，复杂的对象使用 deepEq 函数进行处理
function eq(a, b) {
    // === 结果为 true 的区别出 +0 和 -0
    if (a === b) return a !== 0 || 1 / a === 1 / b;
    // typeof null 的结果为 object ，这里做判断，是为了让有 null 的情况尽早退出函数
    if (a == null || b == null) return false;
    // 判断 NaN
    if (a !== a) return b !== b;
    // 判断参数 a 类型，如果是基本类型，在这里可以直接返回 false
    var type = typeof a;
    if (type !== 'function' && type !== 'object' && typeof b != 'object') return false;
    // 更复杂的对象使用 deepEq 函数进行深度比较
    return deepEq(a, b);
};
function deepEq(a,b){
	if(Array.isArray(a)&& Array.isArray(b)){
		length = a.length;
		if(length !==b.length) return false;
		// 递归遍历
		while(length--){
			if(!eq(a[length],b[length])) return false;
		}
	}
	else throw new TypeError('TypeError')
};
```

#### 1.4.2.2 如何判断两个普通对象相等(键值对及排列顺序完全相等）？

+ 方法1：递归遍历
```js
// 使用上述eq函数
function isSimpleObj(obj){
	return Object.prototype.toString.call(obj)==='[object Object]';
}
function deepEq(a,b){
	if(isSimpleObj(a)&&isSimpleObj(b)){
		var keys = Object.keys(a),key;
		length = keys.length;
		if(Object.keys(b).length!==length) return false;
		while(length--){
			key = keys[length];
			if(!(b.hasOwnProperty(key)&&eq(a[key],b[key]))){
				return false;
			}
		}
	}
}
```
+ 方法2：**JSON.stringify(obj)** 把对象转换成字符串后比较。
	- 优点：用法简单，对于顺序相同的两个对象可以快速进行比较得到结果

> 当两个对比的对象中只有key的顺序不是完全相同时，依然认为“对象相等”，这个时候，不能使用上面的两种方法判断。
>
> **object.keys()** 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。
>
> 详细资料：[JavaScript专题之如何判断两个对象相等](https://github.com/mqyqingfeng/Blog/issues/41) 、 [如何对比js中两个对象是否相等](https://www.jianshu.com/p/90ed8b728975)




------

### 1.4.2 深拷贝与浅拷贝

- 浅拷贝是创建一个新对象，这个对象有着原始对象属性值的一份精确拷贝。如果属性是基本类型，拷贝的就是基本类型的值，如果属性是引用类型，拷贝的就是内存地址 ，所以**如果其中一个对象改变了这个地址，就会影响到另一个对象**。

- 深拷贝是将一个对象从内存中完整的拷贝一份出来,从堆内存中开辟一个新的区域存放新对象,且**修改新对象不会影响原对象**。
```js
var a1 = {b: {c: {}};

var a2 = shallowClone(a1); // 浅拷贝方法
a2.b.c === a1.b.c // true 新旧对象还是共享同一块内存

var a3 = deepClone(a3); // 深拷贝方法
a3.b.c === a1.b.c // false 新对象跟原对象不共享内存
```
> 详细资料：[前端工匠:一文读懂js深拷贝与浅拷贝](https://github.com/ljianshu/Blog/issues/5)

#### 1.4.2.1 原生JS实现浅拷贝
```js
// 【原生实现】
function shallowCopy(source){
	let target = {};
	for(var i in source){
		if(source.hasOwnProperty(i)){
			target[i]=source[i];
		}
	}
}
// 验证
let obj1 = {
	name:'兰尼斯特',
  arr:[1,[2,3],4]
};
let obj2 = shallowCopy(obj1);
obj2.name='拜拉席恩';
obj2.arr[1]=[5,6,7];
console.log('obj1',obj1);
//obj1{name:'兰尼斯特',arr:[1,[5,6,7],4]}
//obj2{name:'拜拉席恩',arr:[1,[5,6,7],4]}
```
------
#### 1.4.2.2 浅拷贝在实践中如何实现？

+ 1. **Object.assign()**方法可以把任意多个源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。
```js
let obj1={
	person:{
		name:'kobe',
		age:41
	},
	sports:'basketball'
};
let obj2= Object.assign({},obj1);
obj2.person.name='wade';
obj2.sports='footbal';
console.log(obj1);
// {person:{name:'wade',age:41},sports:'basketball'}
```

+ 2. 函数库**lodash**的_.clone方法
```js
	var _ = require('lodash');
	var obj1 = {
		a:1,
		b:{
			f:{
				g:1
			}
		},
		c:[1,2,3]
	};
	var obj2 = _.clone(obj1);
	console.log(obj1.b.f===obj2.b.f)//true
```

+ 3. 展开运算符...
	- es6提供的展开运算符也可以实现浅拷贝。
```js
let obj1 = {name:'kobe',address:{x:100,y:110}};
let obj2 = {...obj1}; // 浅拷贝
obj1.address.x=200;
obj1.name='wade';
console.log('obj2',obj2);
//obj2 {name:'kobe',address:{x:200,y:110}}
```

+ 4. 针对数组的浅拷贝
	- **Array.prototype.concat()**
	- **Array.prototype.slice()**
```js
let arr = [1,3,{
	username:'kobe',
}];
let arr2 = arr.concat();
arr2[2].username='wade';
console.log(arr);
//[1,3,{username:'wade'}]
```
```js
let arr = [1,3,{
	username:'kobe',
}];
let arr3 = arr.slice();
arr3[2].username='wade';
console.log(arr);
//[1,3,{username:'wade'}]
```
------
#### 1.4.2.3 深拷贝的原生实现
> 递归思想
```js
//这是一个深拷贝的方法（忽略obj或其属性值是函数的情况）
function deepCopy(obj){
  if(obj===null) return obj;
  if(obj instanceof Date) return new Date(obj);
  if(obj instanceof RegExp) return new RegExp(obj);
  if(typeof obj!=='object' && typeof obj!=='function') return obj;// 暂时这样判断为基本类型
  //!!!
  let cloneObj = new obj.constructor();
  for(let key in obj){
    if(obj.hasOwnProperty(key)){
      // 递归拷贝
      cloneObj[key]=deepCopy(obj[key])
    }
  }
  return cloneObj;
}
// 验证
let obj1 = {
	name:'兰尼斯特',
  arr:[1,[2,3],4]
};
let obj2 = deepCopy(obj1);
obj2.name='拜拉席恩';
obj2.arr[1]=[5,6,7];
console.log('obj1',obj1);
// obj1 {name:'兰尼斯特',arr:[1,[2,3],4]}
console.log('obj2',obj2);
// obj3 {name:'拜拉席恩',arr:[1,[5,6,7],4]}
```
------
#### 1.4.2.4 深拷贝在实践中如何实现？
+ 1. **JSON.parse(JSON.stringify())**
	- 利用JSON.stringify将对象转换成JSON字符串，再用JSON.parse把字符串解析成对象，这样创建的新对象会开辟新的栈，实现深拷贝。这种方法可以实现数组或对象深拷贝，但是不能处理函数和正则。应为这两者基于JSON.stringify和JSON.parse处理后，得到的正则就不再是正则（变为空对象），得到的函数就不再是函数(变为null)。

```js
let arr = [1,3,{
	username:'kobe'
}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'wade';
console.log(arr,arr4);
```
+ 2. 函数库**lodash** 的_.cloneDeep方法
```js
var _ = require('lodash');
var obj1 = {
	a:1,
	b:{
		f:{
			g:1
		}
	},
	c:[1,2,3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f===obj2.b.f)
//false
```

+ 3. 递归实现深拷贝，同时兼顾解决循环引用的问题。

> 详细参考：[如何写出一个惊艳面试官的深拷贝](https://segmentfault.com/a/1190000020255831)
------
### 1.4.3 原型链

#### 1.4.3.1  什么是js中的原型？原型链？有什么特点？

+ 在 js 中我们是使用构造函数来新建一个对象的，每一个构造函数的内部都有一个 prototype 属性值，这个属性值是一个对象，这个对象包含了可以由该构造函数的所有实例共享的属性和方法。当我们使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的 prototype 属性对应的值，在 ES5 中这个指针被称为对象的原型。一般来说我们
  是不应该能够获取到这个值的，但是现在浏览器中都实现了\_\_proto\_\_ 属性来让我们访问这个属性，但是我们最好不要使用这个属性，因为它不是规范中规定的。ES5 中新增了一个**Object.getPrototypeOf() **方法，我们可以通过这个方法来获取对象的原型。

+ 当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是 Object.prototype 所以这就是我们新建的对象为什么能够使用 toString() 等方法的原因。

+ 特点：
  
  - JavaScript 对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。

<div align="center">
	<img src="0_pictures/原型链.webp" alt="1" width="500x">
</div>


> 详细资料可以参考：[《JavaScript 深入理解之原型与原型链》](http://cavszhouyou.top/JavaScript%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3%E4%B9%8B%E5%8E%9F%E5%9E%8B%E4%B8%8E%E5%8E%9F%E5%9E%8B%E9%93%BE.html) 、[前端工匠:原型与原型链详解](https://github.com/ljianshu/Blog/issues/18)

------
#### 1.4.3.2  js中获取原型有哪些方法？

- p.\_\_proto\_\_
- p.constructor.prototype
- **Object.getPrototypeOf(p)**
-------

#### 1.4.3.3 如何判断一个属性是否存在于原型对象上？
+ **obj.hasOwnProperty(prop)** ：只有当属性prop存在于实例对象obj时才返回true。
+ **in** 操作符：单独使用 <span id="in">in操作符</span>时，in操作符会在可以通过对象访问指定属性时返回true，无论这个属性是在实例上还是原型上。
```js
// 判断一个属性是否定义在了原型对象上
function hasPrototypeProperty(object,name){
	// 实例对象上没有该属性 且 in操作符返回true
	return !object.hasOwnProperty(name) && (name in object);
}
```
------
#### 1.4.3.4  Javascript 中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是？

+ Object.prototype.hasOwnProperty.所有继承了 Object 的对象都会继承到 hasOwnProperty 方法。这个方法可以用来检测一个对象是否含有特定的自身属性，和in 运算符不同，该方法会忽略掉那些从原型链上继承到的属性。
------

### 1.4.4 继承的多种方式及优缺点。

+ 1. 以**原型链**的方式实现继承:
	- 这种实现方式存在的缺点是，在包含有引用类型的数据时，原型中的引用值会被所有的实例对象所共享，容易造成修改的混乱。还有就是在子类型实例化时不能向父类型的构造函数传递参数。
```js
function SuperType(){
	this.color = ["red","blue","green"];
}
function SubType(){};
// 继承
SubType.prototype = new SuperType();

let instance1 = new SubType();
instance1.colors.push('black');
console.log(instance1.colors); //"red,blue,green,black"

let instance2 = new SubType();
console.log(intance2.colors); //"red,blue,green,black"
```

+ 2. **盗用构造函数**
	- 这种方式是通过在子类型的函数中调用父类型的构造函数来实现的。这一种方法解决了不能向父类型传递参数的缺点，但是它存在的一个问题就是无法实现函数方法的复用，并且父类型原型定义的方法子类型也没有办法访问到。
```js
function SuperType(name){
	this.name = name;
};
function SubType(){
  // 借用父类构造函数，并在子类构造函数中向父类构造函数传参
	SuperType.call(this,'Nicholas');
	// 调用父类构造函数后，还可以再给子类实例添加额外的属性
	this.age=29;
};
let instance = new SubType();
console.log(instance.name); //'Nicholas'
console.log(instance.age);// 29
```

+ 3. **组合继承**：
	- 组合继承综合了原型链和盗用构造函数，<u>通过盗用构造函数继承实例属性，通过原型对象来继承原型上的属性和方法</u>。组合继承弥补了原型链和盗用构造函数的不足，是javascript中使用最多的继承模式。但是有一点不足的就是，因为使用了两种不同的模式，所以对于代码的封装性不够好。

```js
function SuperType(name){
	this.name = name;
	this.colors = ['red','blue','green'];
}
SuperType.prototype.sayName = function(){
	console.log(this.name);
}
function SubType(name,age){
	// 继承属性
	SuperType.call(this,name);
	this.age=age;
}
// 继承方法
SubType.prototype = new SuperType();
SubType.prototype.sayAge = function(){
	console.log(this.age);
};
let instance1 = new SubType('Nicholas',29);
let instance2 = new SubType('Greg',27);
instance1.colors.push('black')
console.log(instance1.colors); // "red,blue,green,black"
console.log(instance2.colors);// "red,blue,green"
```

+ 4. **原型式继承**
	- 原型式继承的主要思路就是<u>基于已有的对象来创建新的对象</u>，实现的原理是，向函数中传入一个对象，然后返回一个以这个对象为原型的对象。这种继承的思路主要不是为了实现创造一种新的类型，只是对某个对象实现一种简单继承，ES5 中定义的 `Object.create()` 方法就是原型式继承的实现。缺点与原型链方式相同。
```js
function object(o){
  function F(){};
  F.prototype = o;
  return new F();
}
//object 函数会创建一个临时构造函数，将传入的对象赋值给这个构造函数的原型，然后返回这个临时类型的一个实例。
// ES5 通过增加Object.create() 方法将原型式继承的概念规范化了。
let person = {
  name:'Nicholas',
  friends:['Shelby','Court','Van'];
}
let anotherPerson = object(person);
anotherPerson.name = 'Greg';// 增加了一个实例自身上的属性
anotherPerson.friends.push('Rob');// 访问到了原型对象person上的引用类型对象

let yetAnotherPerson = object(person);
yetAnotherPerson.name = 'Linda';
yetAnotherPerson.friends.push('Barbie');

console.log(person.friends) // 'Shelby','Court','Van','Rob','Barbie'
// 属性中包含的引用值始终会在相关对象间共享
```

+ 5. **寄生式继承**
	- 寄生式继承的思路是创建一个用于封装继承过程的函数，通过传入一个对象，然后复制一个对象的副本，然后增强这个对象，最后返回这个对象。这个扩展的过程就可以理解是一种继承。这种模式的缺点是无法实现内部”增强函数“的复用,与构造函数模式类似。
```js
function createAnother(original){
	let clone = object(original);   // 通过调用函数创建一个对象
	clone.sayHi = function(){      // 增强这个对象
		console.log('hi');
	};
	return clone;
};

let person = {
	name:'Nicholas',
	friends:['Shelby','Court','Van']
};
let anotherPerson = createAnother(person);
anotherPerson.sayHi(); // 'hi'
```

+ 6. **✅ 寄生式组合继承** 
	- 组合继承存在效率问题，父类构造函数始终会被调用两次，一次是在创建子类原型的时候调用，另一次是在子类构造函数中调用。
	- 寄生式组合继承的基本思路是使用**父类型的原型**作为子类型的原型,这样只调用了一次父类构造函数，效率更高。`寄生式组合继承`是引用类型继承的最佳模式。

```js
// 实现函数： 将父类原型 赋值给 子类的原型
function inheritPrototype(subType, superType){
	let prototype = object(superType.prototype);
  			// 或者 = Object.create(superType.prototype); //ES5
	prototype.constructor = subType;
	subType.prototype = prototype;
};

function SuperType(name){
	this.name = name;
	this.colors = ['red','blue','green'];
};
SuperType.prototype.sayName = function(){
	console.log(this.name);
};
function SubType(name,age){
	// 父类构造函数只在这里调用一次
	SuperType.call(this,name);
	this.age = age;
};
inheritPrototype(SubType,SuperType);
SubType.prototype.sayAge = function(){
	console.log(this.age);
};
```
------

### 1.4.5 类

#### 1.4.5.1 什么是js的类？有哪些特点？

+ ES6 新引入了**class**关键字具有定义类的能力。类是ES6新增的基础性语法糖结构。
+ 定义类有两种主要方式：类声明和类表达式。
```js
// 类声明
class Person{}
// 类表达式
const Animal = class {} 
```
+ 与函数定义不同，函数声明可以提升，但类定义不能提升。
+ 类受块作用域的限制。
+ 默认情况下，类中定义的代码都在严格模式下执行。
+ 把类表达式赋值给变量后，可以通过name属性获取类表达式的名称。
+ **constructor**关键字用于在类内部定义块内部创建类的构造函数。方法名constructor会告诉解释器使用new操作符创建类的实例时，应该调用这个函数。类实例化时传入的参数会用作构造函数的参数。
+ 通过typeof标识符检测类标识符，表明它是一个函数。（返回`function`）。
+ 使用**instanceof**操作符可以检测构造函数的原型是否出现在实例的原型链上。类标签符也有prototype属性，而这个原型也有一个constructor属性指向类自身。
```js
class Person{};
let p = new Person();
console.log(p instanceof Person);// true 
```
+ 在类的上下文中，类本身在使用new调用时就会被当成构造函数，而类中定义的constructor方法不会被当成构造函数，在对它使用**instanceof**操作符时会返回false。
+ 可以把方法定义在`类构造函数`中或`类块`中。在`类块`中定义的所有内容都会出现在类的原型上。
```js
class Person{
  constructor(){
  // 这里相当于构造函数
    this.locate = ()=>console.log('instance')
  }
  // 这里相当于构造函数的原型对象
  locate(){
    console.log('prototype')
  }
}
```

> 详细资料可以参考：[《ECMAScript 6 实现了 class，对 JavaScript 前端开发有什么意义？》](https://www.zhihu.com/question/29789315)
> [《Class 的基本语法》](http://es6.ruanyifeng.com/#docs/class)


------

#### 1.4.5.2 如何判断一个对象是否属于某个类？

+ 1. 使用 **instanceof** 运算符来判断构造函数（类）的 prototype 属性是否出现在对象的原型链中的任何位置。
+ 2. 通过对象的 constructor 属性来判断: 对象的 constructor 属性指向该对象的构造函数，但是这种方式不是很安全，因为 constructor 属性可以被改写。
+ 3. 如果需要判断的是某个内置的引用类型的话，可以使用 Object.prototype.toString() 方法来打印对象的[[Class]] 属性来进行判断。

> 详细资料可以参考：[《js 判断一个对象是否属于某一类》](https://blog.csdn.net/haitunmin/article/details/78418522)
------
#### 1.4.5.3 如何判断一个原型对象是否出现在一个实例对象的原型链上？
+ 1. 使用**`instanceof`**操作符，如果这个原型对象出现在实例对象原型链上，则返回true。
```js
console.log(instance instanceof Obejct); // true
console.log(instance instanceof SuperType); //true
console.log(instance instanceof SubType); // true
```
+ 2. 使用**`isPrototypeOf()`**方法。原型链中的每个原型对象都可以调用这个方法，只要实例的原型链中包含这个原型对象。
> `isPrototypeOf()`方法定义在`Object.prototype`对象上。
```js
console.log(Object.prototype.isPrototypeOf(instance)); // true
console.log(SuperType.prototype.isPrototypeOf(instance)); // true
console.log(SubType.prototype.isPrototypeOf(instance)); // true
```
------

### 1.4.6 Set和Map 数据结构

------

#### 1.4.6.1 比较Set和WeakSet结构？
+ 1. ES6 提供了新的数据结构 Set。它类似于数组，但是**成员的值都是唯一的**，没有重复的值。
+ 2. WeakSet 结构与 Set 类似，也是不重复的值的集合。但是 WeakSet 的**成员只能是对象**，而不能是其他类型的值。WeakSet 中的对象都是**弱引用**，即垃圾回收机制不考虑 WeakSet 对该对象的引用。

------

#### 1.4.6.2 比较Map和WeakMap结构
+ 1.Map 数据结构。它类似于对象，也是键值对的集合，**但是“键”的范围不限于字符串**，各种类型的值（包括对象）都可以当作键。
+ 2.WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合。但是 WeakMap **只接受对象作为键名（ null 除外）**，不接受其他类型的值作为键名。而且 WeakMap 的键名所指向的对象，不计入垃圾回收机制。

> 详细参考：[阮一峰：Set和Map数据结构](https://es6.ruanyifeng.com/#docs/set-map)
------
## 1.5 数组

### 1.5.1 <span id="ArrayMethods">数组有哪些原生方法？</span>

> 以下红色加粗或显示带🖍的方法的表示这些方法会直接改变原数组。

#### 1.5.1.1 增

+ <font color=red>**push()**</font> 🖍方法接收任意数量的参数，并将它们添加到数组的末尾，返回数组的最新长度。
```js
let colors = ['black'];
let count = colors.push('red','blue');
console.log(count);// 3  
console.log(colors);// ['black','red','blue']
```

+ <font color=red>**unshift()**</font>🖍方法在数组开头添加任意多个值，然后返回新数组的长度。
```js
let colors = ['black'];
let count = colors.unshift('red','blue');
console.log(count);// 3  
console.log(colors);// ['red','blue','black']
```

+ <font color=red>**splice()**</font>🖍方法传入三个参数，分别是开始位置、要删除的元素的数量、插入的元素。**splice()**方法会返回被删除的元素组成的数组，如果没有元素被删除，则返回空数组。
```js
// (只增未删)
let colors = ["red","green","blue"];
let removed = colors.splice(1,0,"yellow","orange");
console.log(colors);// ["red","yellow","orange","green","blue"]
console.log(removed);//[] 
```

+ <font color=green>**concat()**</font> 方法首先会创建一个当前元素的副本，然后再把它的参数添加到副本的末尾，最后返回新构建的数组。
```js
let colors = ["red","green","blue"];
let colors2 = colors.concat("yellow",["brown","black"]);
console.log(colors);// ["red","green","blue"]
console.log(colors2);// ["red","green","blue","yellow","brown","black"]
```

#### 1.5.1.2 删

+ <font color=red>**pop()**</font>🖍用于删除数组的最后一项，同时减少数组的length值，返回被删除的项。
```js
let colors = ["red","blue"];
let item = colors.pop();
console.log(item);// ["blue"] 被删除项
console.log(colors);//["red"]
```

+ <font color=red>**shift()**</font>🖍 用于删除数组的第一项，同时减少数组的length值，返回被删除的项。
```js
let colors = ["red","blue"];
let item = colors.shift();
console.log(item);// ["red"] 被删除项
console.log(colors);//["blue"]
```

+ <font color=red>**splice()**</font>🖍 传入两个参数，分别是开始位置，删除元素的数量，返回包含删除元素的数组。（相当于第三个参数不传）
```js
// （只删未增）
let colors = ["red","green","blue"];
let removed = colors.splice(1,1); // 删除第二项
console.log(colors);//["red","blue"]
console.log(removed);//["green"] 
```
+ <font color=green>**slice()**</font> 用于创建一个包含原有数组的一个或多个元素的新数组，不改变原数组。<font color=green>**slice()**</font>方法接收一个或两个参数：开始索引和结束索引。如果只有一个参数，就是返回该索引到数组末尾的所有元素。如果有两个参数，则返回从开始索引到结束索引的所有元素，其中不包含结束索引对应的元素。
```js
let colors=["red","green","blue","yellow","purple"];
let colors2=colors.slice(1);
let colors3=colors.slice(1,4);
console.log(colors);//["red","green","blue","yellow","purple"]
console.log(colors2);//["green","blue","yellow","purple"]
console.log(colors3);//["green","blue","yellow"]
```

#### 1.5.1.3 改
+ <font color=red>**splice()**</font>🖍方法传入三个参数，分别是开始位置、要删除的元素的数量、插入的元素。**splice()**方法会返回被删除的元素组成的数组，如果没有元素被删除，则返回空数组。
```js
// （改）
let colors = ["red","green","blue"];
let removed = colors.splice(1,1,"yellow","orange");
console.log(colors);// ["red","yellow","orange","blue"]
console.log(removed);//["green"] 
```

+ <font color=red>**fill()**</font>🖍方法使用一个给定值填充一个数组中从起始索引到终止索引内的全部元素，不包括终止索引。fill方法会改变原数组。第二和第三个参数可选。
```js
const array1 = [1, 2, 3, 4];
console.log(array1.fill(0, 2, 4));
// [1, 2, 0, 0]
console.log(array1);
// [1, 2, 0, 0]

// fill 方法用于空数组的初始化非常方便。
const arr = new Array(3).fill(9);
console.log(arr);
//[9,9,9]
```

#### 1.5.1.4 查

+ **严格相等**匹配查找：
  - <font color=green>**indexOf()**</font> 和<font color=green>**lastIndexOf()**</font>方法返回要查找的元素在数组中的位置，如果没找到，返回-1。前者从前往后找，后者从后往前找。它们都接收两个参数：要查找的元素和一个可选的起始位置。
  - <font color=green>**includes()**</font>方法返回一个布尔值，表示是否至少找到一个与指定元素严格相等的项。接收两个参数：要查找的元素和一个可选的起始位置。

```js
let numbers = [1,2,3,4,5,6,7];
console.log(numbers.indexOf(4)); //3
console.log(numbers.lastIndexOf(2));//5
console.log(numbers.includes(7));//true
```

+ 按照**自定义断言函数匹配**查找：

  - <font color=green>**find()**</font>和<font color=green>**findIndex()**</font>方法使用了`断言函数`。这两个方法接收断言函数作为参数，从数组的最小索引开始。find()返回<u>第一个匹配到的元素</u>，findeIndex() 返回<u>第一个匹配到的元素的索引</u>。这两个方法也都接收第二个可选的参数，用于指定断言函数内部的this的值。找到匹配项后，就不再继续搜索。
  - **断言函数**: `断言函数接`收三个参数，`元素`，`索引` 和`数组`本身。断言函数返回真值，表示匹配到了要搜寻的元素。
```js
const people = [
	{
		name:"Matt",
		age:27
	},
	{
		name:'Nicholas',
		age:'30'
	}
];
let item = people.find((elem,index,arr)=>elem.age<28);
console.log(item);// {name:'Matt',age:27}
let itemIndex = people.find((elem,index,arr)=>elem.age<28);
console.log(itemIndex);//0
```
------

#### 1.5.1.5 数组的排序

+ <font color=red>**reverse()**</font>🖍方法会将数组元素反向排列。
```js
let value = [1,2,3,4,5];
values.reverse();
console.log(values);//[5,4,3,2,1]
```

+ <font color=red id="sort">**sort()**</font>🖍 方法会默认按照每一项元素使用String()转型函数转换后，比较字符串来决定顺序。<font color=red>**sort()**</font> 方法可以接收一个比较函数作为参数，用于判断哪个值应该排在前面。比较函数接收两个参数，如果第一个参数应该排在第二个参数前面，就返回负值；如果两个参数相等就返回0；如果第一个参数应该排在第二个参数后面，就返回正值。如下是一个简单的适用于大多数数据类型的比较函数。
```js
let values = [0,1,5,2,10,6,7];
// 升序
values.sort((a,b)=>a<b?-1:a>b?1:0);
console.log(values);//[0,1,2,5,6,7,10]
// 降序
values.sort((a,b)=>a<b?1:a>b?-1:0);
console.log(value);//[10,7,6,5,2,1,0]
```

#### 1.5.1.6 数组的转换

+ <font color=green>**join()**</font>方法接收一个参数，即字符串分隔符，返回包含所有项的字符串。
```js
let colors = ["red","green","blue"];
console.log(colors.join('*'));
//red*green*blue
```

------

#### 1.5.1.7 数组的迭代

>  ES为数组定义了五个迭代方法。每个方法接收两个参数：以每一项为参数运行的函数，以及可选的作为函数运行上下文的作用域对象。传给每个方法的函数接收三个参数：数组元素、元素索引和数组本身。

+ <font color=green>**some()**</font> 对数组的每一项都运行传入的函数，如果**有一项**函数返回true，则这个方法返回true。
```js
let numbers = [1,2,3,4,5];
let result = numbers.some((item,index,array)=>item>2);
console.log(result); // true
```

+ <font color=green>**every()**</font> 对数组的每一项都运行传入的函数，如果**对每一项**函数返回true，则这个方法返回true。
```js
let numbers = [1,2,3,4,5];
let result = numbers.every((item,index,array)=>item>2);
console.log(result); // false
```

+ <font color=green id="forEach">**forEach()**</font> 对数组的每一项都运行传入的函数，**没有返回值**。
```js
let numbers = [1,2,3,4,5];
numbers.forEach((item,index,array)=>{
  console.log(item*2)
});
//2 4 6 8 10
```
+ <font color=green>**filter()**</font> 对数组的每一项都运行传入的函数，函数返回true的项，会组成数组之后返回。
```js
let numbers = [1,2,3,4,5];
let result = numbers.filter((item,index,array) => item>2);
console.log(result);
//[3,4,5]
```
+ <font color=green>**map()**</font> 对数组的每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。
```js
let numbers = [1,2,3,4,5];
let result = numbers.map((item,index,array)=>item*2);
console.log(result);
//[2,4,6,8,10]
```
#### 1.5.1.8 数组的归并

+ ES为数组提供了两个归并方法。<font color=green>**reduce()**</font> 和 <font color=green>**reduceRight()**</font> 。 这两个方法都接收两个参数，对每一项都会运行的归并函数，以及可选的以之为归并起点的初始值。归并函数接收4个参数：上一个归并值，当前项，当前项的索引和数组本身。归并函数执行一次返回的任何值都将作为下一次归并调用时的第一个参数。如果没有给这两个方法传入第二个参数（作为归并起点值），则第一次迭代将从数组的第二项开始，因此传给归并函数的第一个参数是数组的第一项，第二个参数是数组的第二项。
```js
// 归并求和
let values = [1,2,3,4,5,6];
// 归并起点第一项为数组的第一项
let sum = values.reduce((prev,cur,index,array)=>prev+cur);
// 归并起点第一项为0
let sum0 = values.reduce((prev,cur,index,array)=>prev+cur,0); 
console.log(sum,sum0);// 21 21
```

### 1.5.2 数组的应用场景

#### 1.5.2.1 数组去重

+ 1. 原始双层循环
```js
var array = [1,1,'1','1'];

function unique(array){
  // res 用来储存结果
  var res = [];
  for(var i=0;i<array.length;i++){
    for(var j=0;j<res.length;j++){
      if(array[i]===res[j]){
        break;// 结束当前内层for循环
      }
    };
    // j===res.length 说明针对每个i，res数组里的每一项都被检查过了一次
    // 并且都不和array[i] 相等，这个时候可以把array[i]放入res数组里
    if(j===res.length){
      res.push(array[i])
    }
  }
  return res;
}
console.log(unique(array));
//[1,'1']
```

+ 2. 🤩**indexOf** 简化内层循环
```js
var array = [1,1,'1','1'];
function unique(array){
	res = [];
	for(var i=0;i<array.length;i++){
		var current = array[i];
    // 针对要被迭代的每一项，检查它能否在res数组里查找到索引
    // 查不到索引说明当前的res数组还没有这一项
		if(res.indexOf(current)===-1){
			res.push(current);
		}
	};
	return res;
}
console.log(unique(array));//[1,'1']
```

+ 3. filter 简化外层循环(迭代)
```js
var array = [1,2,1,1,'1'];
function unique(array){
	var res = array.filter((item,index,array)=>{
    // 针对数组的每一项查找索引，重复的项会查到多个索引，会被过滤
    return array.indexOf(item)===index;
  });
  return res;
};
console.log(unique(array)) //[1,2,'1'];
```

+ 4. ES6 set数据结构
> Set 是一种类数组的数据结构，但是成员都是唯一的。Set构造函数可以接收一个数组或类数组作为参数，用来初始化。Set数据结构可以被扩展符展开。
```js
var array = [1,1,2,2,'1'];
function unique(array){
	return Array.from(new Set(array))
}
console.log(unique(array)) //[1,2,'1'];
```
```js
// 或者
function unique(array){
	return [...new Set(array)]
}
```
> 详细参考: [Javascript专题之数组去重](https://github.com/mqyqingfeng/Blog/issues/27)
------

#### 1.5.2.2 数组扁平化

> 数组扁平化就是将一个嵌套多层的array转换为只有一层的数组。

+ 1. 递归
```js
var arr = [1,[2,[3,4]]];
function flatten(arr){
  var result = [];
  for(var i=0;i<arr.length;i++){
    //如果某一项是数组，使用concat方法拼接递归后的这一项
    if(Array.isArray(arr[i])){
      result = result.concat(flatten(arr[i]))
    }else{
      result.push(arr[i])
    }
  }
  return result;
}
console.log(flatten(arr));
// [1,2,3,4]
```
+ 2. reduce归并
```js
var arr = [1,[2,[3,4]]];
function flatten(arr){
	return arr.reduce(function(prev,cur){
		return prev.concat(Array.isArray(cur)?flatten(cur):cur)
	},[])
}
console.log(flatten(arr));
// [1,2,3,4]
```

+ 3. while循环 
```js
var arr = [1,[2,[3,4]]];
function flatten(arr){
	while(arr.some(item=>Array.isArray(item))){
		arr=[].concat(...arr);
	};
	return arr;
}
console.log(flatten(arr));
// [1,2,3,4]
```

------

#### 1.5.2.3 求数组的最大（最小值）

> 这里以最大值为例。
> 基于静态方法（Math.max(参数列表))

+ 1. 原始循环遍历
```js
var arr = [6,3,1,4,7,5,2,10]

function maxArr(arr){
	var result = arr[0];
	for(var i=1;i<arr.length;i++){
		result = Math.max(result,arr[i]);
	};
	return result;
};
console.log(maxArr(arr));//10
```

+ 2. reduce遍历
```js
var arr =[6,4,1,8,2,11,23]
function maxArr(arr){
	return arr.reduce((prev,cur)=>Math.max(prev,cur));
};
console.log(maxArr(arr));//23
```

+ 3. sort 排序
```js
var arr =[6,4,1,8,2,11,23];
function maxArr(arr){
	// 降序
	var newArr = arr.sort((a,b)=>b-a);
	return newArr[0];
}
console.log(maxArr(arr));//23
```

+ 4.apply
```js
var arr = [6,4,1,8,2,11,23];
function maxArr(arr){
	return Math.max.apply(null,arr);
};
console.log(maxArr(arr));//23
```

+ 5. ES6 扩展运算符
```js
var arr = [6,4,1,8,2,11,23];
function maxArr(arr){
	return Math.max(...arr);
};
console.log(maxArr(arr));//23
```

------

#### 1.5.2.4 在数组中查找指定元素

> ES6对数组新增了findIndex方法，会返回满足提供的函数的第一个元素的索引，否则返回-1

```js
function isBigEnough(element){
	return element>10;
}
console.log([4,10,23,19].findIndex(isBigEnough));//2
```
+ 如何实现findIndex?
```js
Array.prototype.myFindIndex = function(fn,context=null){
  //this 指向调用此函数的数组对象
	for(var i=0;i<this.length;i++){
    if(fn.call(context,this[i],i,this)) return i;
  }
  return -1;
}
console.log([4,10,23,19].myFindIndex(isBigEnough));//2
```
> 详细资料可以参考：[JavaScript专题之学underscore在数组中查找指定元素](https://github.com/mqyqingfeng/Blog/issues/37)

------

#### 1.5.2.5 如何将数组打乱？

> 基于Math.random() 函数和[sort](#sort)方法
```js
var values =[1,2,3,4,5,6];
values.sort(function(){
	return Math.random()-0.5;
});
console.log(values);
```
> 更加精确的乱序算法参考[JavaScript专题之乱序](https://github.com/mqyqingfeng/Blog/issues/51)
------

#### 1.5.2.6 比较数组的各种迭代方法？

+ 1. **for循环**
	- for 循环的遍历方式与另外两者的差别是最大的，通过代码块来执行循环。在代码块中，需要通过迭代变量来获取当前遍历的元素，如`arr[i]`。
	 - 看上去通过迭代变量获取元素没有另外两种方式（能够直接获取）方便，但是在某些情况下，我们却不得不使用 for 循环：**当在循环满足特定条件时跳出循环体，或跳出本次循环直接进行下一次循环。**
```javascript
let arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
for (let i = 0; i < arr.length; i++) {
  // 当迭代变量 i == 3 时，跳过此次循环直接进入下一次
  if (i == 3) continue;
  console.log(arr[i]);
  // 当 i > 7 时，跳出循环
  if (i > 7) break;
}

//>> 0
//>> 1
//>> 2
//>> 4
//>> 5
//>> 6
//>> 7
//>> 8
```

另外两种遍历方式，由于是通过回调函数的方式对遍历到的元素进行操作，即使在回调函数中 return ，也仅能够跳出当前的回调函数，无法阻止遍历本身的暂停。

```javascript
let arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
arr.forEach(item => {
  console.log(item);
  if (item > 3) return; // 遍历并没有在大于 3 时结束
});

//>> 0
//>> 1
//>> 2
//>> 3
//>> 4
//>> 5
//>> 6
//>> 7
//>> 8
//>> 9
```

+ 2. **forEach**

	- **`forEach()`** 方法对数组的每个项执行一次提供的回调函数。
	- **语法如下：**
```javascript
arr.forEach(callback[, thisArg]);
```
> 参数说明: [forEach](#forEach)

```javascript
var a = "coffe";
var b = {a:"1891"};
(function() {
  let arr = [0, 1, 2];
  arr.forEach(function(item){
    console.log(this.a);//这里是访问的b.a
  },b);//这里把b作为thisArg参数传入之后，this就指向了b
})();

//>> 1891
//>> 1891
//>> 1891
```

> **注 意：**  
> 如果使用箭头函数表达式来传入`thisArg` 参数会被忽略，因为箭头函数在词法上绑定了 `this` 值。

```javascript
var a = "coffe";
var b = {a:"1891"};
(function() {
  let arr = [0, 1, 2];
  arr.forEach((item)=>{
    console.log(this.a);//这里是访问的window.a
  },b);//这里把b作为thisArg参数传入之后，本来this就应指向b，但由于使用了箭头函数表达式，
       //this固定指向包含它的函数的外层作用域（也即匿名函数）的this，也即window
})();

//>> coffe
//>> coffe
//>> coffe
```

+ 3. **map**
	- map 的使用与 forEach 几乎一致，唯一的区别是：**map 会返回一个新的数组，而这个数组的元素是回调函数的返回值**，所以我们可以用一个变量接收 map 的返回值。

```javascript
let arr = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
const arr1 = arr.map(item => item + 1);
console.log(arr1);
//>> [1,2,3,4,5,6,7,8,9,10]

const arr2 = arr.map(item => {item + 1});//注意这个回调箭头函数并没有返回值
console.log(arr2);
//输出一个数组项都为undefined的数组
//>> [undefined, undefined, …… undefined]
```

> 上面的代码中，`arr1`将回调函数的返回值`item + 1`作为了数组中的元素，而`arr2`由于回调函数没有返回值，所以创建了一个每项都为`undefined`的数组。

+ 4. **for...in** 与**for...of**

	- for...in用来遍历数据结构的”键“。for...in 可以遍历数组，但特别适合遍历对象。
	
	- for...of用来遍历数据结构的”值“，而不包括数组的原型属性、方法，以及索引。for...of可以用来遍历数组，不能直接遍历对象。
	
> for...of 只能用来遍历具有**Symboli.iterator**属性的数据结构。数组原生具备Iterator接口，可以直接用for...of 循环遍历。而对象并没有部署Iterator接口，是因为对象属性的遍历先后顺序是不确定的，需要开发者手动指定。
>
> for...of循环内部调用的是数据结构的Symbol.iterator方法。原生具备Symbol.iterator接口的数据结构都部署了遍历器接口。如：Array，Map，Set，String，TypedArray，arguments对象，NodeList对象。

```javascript
Array.prototype.getLength = function () {
   return this.length;
}
var arr = ['a','b', 'c', 'd','e','f','g'];
arr.name = "coffe1981";
console.log("-------for...of--------");
for (var value of arr) {
   console.log(value);
}
console.log("-------for...in--------");
for (var key in arr) {
   console.log(key);
}

//>>    -------for...of--------
//>>    a
//>>    b
//>>    c
//>>    d
//>>    e
//>>    f
//>>    -------for...in--------
//>>    0
//>>    1
//>>    2
//>>    3
//>>    4
//>>    5
//>>    name
//>>    getLength
```

如上代码，会发现 for...in 可以遍历到原型上的属性和方法，参考[in操作符](#in)。如果不想遍历原型的属性和方法，则可以在循环内部用`hasOwnPropery`方法判断某属性是否是该对象的实例属性。

```javascript
Array.prototype.getLength = function () {
   return this.length;
}
var arr = [1, 2, 4, 5, 6, 7]
arr.name = "coffe1981";
console.log("-------for...in--------");
for (var key in arr) {
	// 只遍历实例上的属性
   if(arr.hasOwnProperty(key))
      console.log(key);
}
//>>    -------for...in--------
//>>    0
//>>    1
//>>    2
//>>    3
//>>    4
//>>    5
//>>    name
```

+ 5. 总结：
	- for..of适用遍历数组/类数组对象/字符串/map/set等拥有迭代器对象的集合，但是不能遍历对象，因为没有迭代器对象。遍历对象通常用for...in来遍历对象的键名。
	- 与forEach不同的是，for...of和for...in都可以正确响应break、continue和return语句。

------
>详细资料参考：[强大的数组](https://github.com/coffe1891/frontend-hard-mode-interview/blob/master/1/1.2.9.md)
------
## 1.6 深入函数

### 1.6.1 函数的参数与arguments对象

+ ES中的函数既不关心传入的参数的个数，也不关心参数的数据类型，之所以会这样是因为ES函数中的参数在内部表现为一个数组，函数被调用时，总会接收一个数组，但函数并不关心这个数组里面包含什么。
+ 使用function关键字定义的函数，可以在函数内部访问`arguments`对象，从中获取传进来的每个参数的值。arguments反映了调用时提供的参数。arguments对象是个类数组对象，因此可以使用中括号语法访问其中的元素。
+ 可以通过arguments对象的`length`属性检查（函数执行时）传入的参数的个数。
+ arguments对象里面的序号属性值始终和对应的命名参数保持同步。<u>但并不意味着它们都能访问同一个内存地址，它们在内存中还是分开的，只不过会保持同步而已</u>。另外，如果只传了一个参数，然后把arguments[1]设置为某个值，那么这个值不会反应到第二个命名参数。这是因为arguments对象的长度是根据函数调用时传入的参数的个数，而非定义函数时给出的命名参数的个数确定的。
```js
function add(num1,num2){
	console.log(arguments[0],arguments[1]) 
	arguments[1]=10;
	console.log(arguments[0]+num2); 
}
add(1,3); // 1,3    11
add(1);  // 1,undefined  NaN
```
+ arguments对象里面有一个**callee**属性，是一个指向arguments对象所在函数的指针。针对<span id="recursion">递归调用的函数</span>，使用`arguments.callee`来代替外层函数名，可以让函数逻辑和函数名**解耦**。

```js
// 经典的阶乘函数是一个递归函数
function factorial(num){
	if(num<=1){
		 return 1;
	}else{
	// return num*factorial(num-1);
	   return num*arguments.callee(num-1)	;
	}
}
```

-------
### 1.6.2  函数的一些属性

+ **length** : 保存函数定义的命名参数的个数。
+ prototype ：保存引用类型所有实例方法的地方。
+ **caller** : 这个属性引用调用当前函数的函数，如果是全局作用域调用则为null。ES5新增。
```js
function outer(){
	inner();
}
function inner(){
  //console.log(inner.caller)
	console.log(arguments.callee.caller);
}
outer();//f outer
```
+ **new.target** : ES6 新增了是否使用new关键字调用的new.target属性。如果是正常调用，该属性的值是undefined，如果使用关键字调用，该属性将引用被调用的构造函数。
```js
function King(){
	if(!new.target){
		throw 'King must be instantiated using "new"'
	}
	consnole.log('King instantiated using "new"')
};
new King();  //King instantiated using "new"
King();  //  Error: King must be instantiated using "new"
```

### 1.6.3 箭头函数

+ ES6 新增了使用 `=>`语法定义函数表达式的能力。
+ 特别注意，由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号()。
```js
var getTempItem = id => ({ id:id, name:"Temp"});
```
+ 箭头函数不能使用arguments、super和new.target ,也不能用作构造函数。此外，箭头函数也没有prototype属性。
+ 箭头函数不可以使用yeild命令，因此箭头函数不能使用Generator函数。

+ ✅ **箭头函数与this**：
	- 箭头函数体内的this总是指向**函数定义生效时所在的对象**。
	- 箭头函数可以让this的指向固定化，这种特性有利于封装回调函数。
	- 箭头函数根本没有自己的this，导致内部的this就是外层代码块的this。正是因为没有this，所以他不能用作构造函数。
	- 由于箭头函数没有自己的this，当然也不能用call、apply和bind这些方法去改变this的指向。

```js
function Timer(){
	this.s1 = 0;
	this.s2 = 0;
	setInterval(
		//箭头函数：this绑定了定义时所在的作用域，即Timer函数
		()=>this.s1++,
		1000);
	setInterval(
	  // 普通函数：this指向运行时所在的作用域（即全局对象window）
		function(){
			this.s2++;
		},1000);
}
var timer = new Timer();
setTimeout(()=>console.log('s1:',timer.s1),3100);
setTimeout(()=>console.log('s2:',timer.s2),3100);
// 3
// 0
// 3100ms后，timer.s1 被更新了三次，而timer.s2 一次也没有更新。
```

```js
(function(){
	return [
		(()=>this.x).bind({x:'inner'})()
	]
}).call({x:'outer'});
// outer
// 箭头函数的this指向定义时所在的作用域，即匿名函数function的作用域。箭头函数使用bind无效。function匿名函数使用call，改变了其内部this的指向，指向了对象{x:'outer'}
```
------
#### 1.6.3.1 [面试题]🚀 箭头函数与普通函数的区别？
+ （待填坑）
------
### 1.6.4 尾调用优化

#### 1.6.4.1 什么是尾调用？

+ 尾调用是函数式编程的一个重要概念。是指某个函数的最后一步是返回**纯粹的另一个函数的调用**。

```js
function f(x){
	return g(x);
}
```

#### 1.6.4.2 什么是尾调用优化？

+ 诸如递归之类的代码很容易在栈内存中产生大量的栈帧，ES6新增了一项内存管理优化机制，使js引擎在满足条件时可以重用栈帧。这项优化非常适合尾调用。所以，在ES6中，只要使用尾递归，就不会发生栈溢出，相对节省内存。

```js
function f(){
	let m =1 ;
	let n =1 ;
	return g(m+n);
}
f();

// 等同于
g(3);
```
+ **尾调用优化**，即只保留内层函数的调用帧。如果所有的函数都是尾调用，那么完全可以做到每次执行时调用帧只有一项，这将大大节省内存。这就是尾调用优化的意义。
+ 尾调用优化的条件：

  - 代码在严格模式下执行；
  - 外部函数的返回值是对尾调用函数的调用。
  - 尾调用函数返回后不需要执行额外的逻辑。
  - 尾调用函数不是引用外部函数作用域中自由变量的闭包。

#### 1.6.4.3  什么是递归？

+ **递归**函数通常的形式是一个函数通过名称调用自己。比如经典的<a href="#recursion">阶乘递归函数</a>
+ [面试题1] ~ 使用递归求10的阶乘
```js
function factorial(num){
  if(num===1){
    return 1;
  }
  return num*factorial(num-1);
}
console.log(factorial(10));
```
+ [面试题2] ~ 使用递归求0~100的累加和
```js
function sum(num){
  if(num===0){
   return 0;
  }
  return num+sum(num-1)
}
```
+ [面试题3] ~ 用一行代码实现斐波那契数列
```js
// 1 1 2 3 5 8 13 21...
function fn(n){
  if(n===1 || n ===2){
    return 1;
  }
  return fn(n-1)+f(n-2)
}
// 1行代码
function fn(n){
  return (n===1||n===2)?1:fn(n-1)+f(n-2);
}
```
#### 1.6.4.4 什么是尾递归？

+ 函数调用自身称为递归，如果函数尾调用自身就称为**尾递归**。尾递归是尾调用的特殊情况。
+ 对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”的错误。
+ 使用尾递归优化[经典阶乘递归函数](#recursion)。
```js
function factorial(n,total){
	if(n===1) return total;
	return factorial(n-1,n*total)
}
factorial(5,1)//120
// 递归过程
// f(5,1)
// f(4,5*1)
// f(3,4*5*1)
// f(2,3*4*5*1)
// f(1,2*3*4*5*1)
// return 2*3*4*5*1
```

+ 使用尾递归优化斐波那契数列（Fibonacci）
```js
function Fibonacci(n){
  if(n<=1) {return 1};
  return Fibonacci(n-1)+Fibonacci(n-2);
}
Fibonacci(10);//89
Fibonacci(100);//堆栈溢出

// 尾递归优化后的Fibonacci数列：
function Fibonacci2(n,ac1=1,ac2=1){
  if(n<=1){return ac2};
  return Fibonacci2(n-1,ac2,ac1+ac2)
}
Fibonacci2(100)// 可求值
Fibonacci2(1000) //可求值
Fibonacci2(10000)//Infinity
```

+ 将尾递归函数[柯里化](#curry)可以将多参数的函数转换成单参数的函数。

```js
// 简单定义一个仅限本例使用的柯里化函数
function curry(fn,n){
	return function(m){
		return fn.call(this,m,n)
	}
}
// 尾递归阶乘函数
function tailFactorial(n,total){
	if(n===1) return total;
	return tailFactorial(n-1,n*total);
}
// 将尾递归函数柯里化
const factorial = curry(tailFactorial,1);
factorial(5)//120

// ---------------
// 上述特例的实现过程也可以使用ES6的参数默认值来实现
function factorial_es6(n,total=1){
  if(n===1) return total;
  return factorial_es6(n-1,n*total);
}
factorial_es6(5)
```
------


### 1.6.5 立即执行函数

+ 立即调用的匿名函数被称作立即调用的函数表达式（IIFE）。
+ 使用IIFE可以创建`块级作用域`。
```js
(function(){
	for(var i=0;i<5;i++){
		console.log(i);
	}
})();
// 0 1 2 3 4
console.log(i);
// ReferenceError : i is not defined
```
+ ES6之后，出现了支持`块级作用域`的变量声明，IIFE就没那么必要了。
```js
for(let i=0;i<5,i++){
	console.log(i);
};
//0 1 2 3 4
console.log(i) 
//ReferenceError : i is not defined
```
> 关于块级作用域，es3中规定的try/catch的catch分句也会创建一个块级作用域！
------

### 1.6.6 函数柯里化

+ **函数柯里化**是针对函数参数而进行优化的技术。

+ <span id="curry">函数柯里化</span>指的是将能够接收多个参数的函数转化为接收单一参数的函数，并且返回一个新函数。这个新函数接收余下的参数且返回结果。

+ 函数柯里化的主要作用和特点就是参数复用、提前返回和延迟执行。

+ 函数柯里化让程序有了更高的自由度，但柯里化使用到了`arguments对象`，`递归`，`闭包`等，频繁使用会给性能带来影响，只有在情况变得复杂时，才是柯里化大显身手的时候。

------

```js
// 理解柯里化
/**
function say(company,department,name){
  console.log(`我是${company}公司，​${department}部门的${name}`);
}
say('Tencent','前端','小王');
say('Tencent','前端','大王');
say('Tencent','前端','老王');
**/
// 【傻瓜式柯里化】
function say(company){
  return function(department){
    return function(name){
      console.log(`我是${company}公司，${department}部门的${name}`);
    }
  }
}
let introd = say('Tencent')('前端');
introd('小李');
introd('老李');
```


------
#### 1.6.6.1 如何封装一个通用的柯里化函数？
> 柯里化一个目标函数，返回一个新函数。

```js
// 柯里化curry.
// fn 表示 要被柯里化的“目标函数”
function curry(fn){
  // 记录fn函数的参数的个数
  let len = fn.length;
  return function temp(){
    // 收集本地实际传参
    let args =[...arguments];
    if(args.length>=len){ // 传参完成
      // 表示只传递了“一次”参数 ,就占满了fn函数的参数坑
     return fn(...args);
    }else{ 
      // 【递归】
      return function(){
        // -- args表示之前实际传入的参数（数量不足）
        // -- arguments表示之后传入的参数
        return temp(...args,...arguments)
      }
    }
  }
}
```
------
```js
// 使用这个柯里化函数
let introd = curry(say);
introd('ByteDance','后端','小张') // ✅
introd('Alibaba', '保洁')('小赵') // ✅
introd('MeiTuan')('骑手')('小吴') // ✅
```


------



## 1.7 Promise 与异步

### 1.7.1定时器

#### 1.7.3.1 setTimeout()

+ **setTimeout()**用于指定在一定时间后执行某些代码，它接收两个参数，要执行的代码和在执行回调函数前等待的时间（ms）。也可以接受第三个参数作为附加参数，一旦定时器到期，会作为参数传递给第一个参数函数。
+ `setTimeout()`函数调用后会返回一个表示该超时排期的数值ID，这个ID是被排期执行代码的唯一标识符，可用于取消该任务，要取消等待中的排期任务，可以调用clearTimeout()方法并传入超时ID。只要在超时时间到达之前调用clearTimeout()方法就可以取消超时任务。
```js
let timeoutId = setTimeout(()=>alert('hello world'),1000);
// 取消超时任务
clearTimeout(timeoutId);
```
------
#### 1.7.3.2 setInterval()

+ **setInterval()**与`setTimeout`类似，只不过指定的任务会每隔一段时间就执行一次，直到取消循环定时或页面卸载。它接收两个参数，要执行的代码和下一次执行任务要等待的时间（ms）。setTimeout()方法也可以返回一个循环定时ID，可以用来在未来某个时间点上取消循环定时。
+ 相对于`setTimeout()`而言，取消定时的能力对setInterval()更重要，如果一直不管它，那么定时任务会一直执行到页面卸载。
+ **setInterval()**在实践中很少在生产环境下使用，因为一个任务和下一个任务开始之间的时间间隔是无法保证的，有些循环定时任务因此可能会被跳过。一般来说最好不要使用setInterval()。
------
#### 1.7.3.3 定时器函数里的this

+ 由setTimeout()调用的代码运行在与所在函数完全分离的执行环境上。这会导致，这些代码中包含的 this 关键字在非严格模式会指向 window (或全局)对象，严格模式下为 undefined，这和所期望的this的值是不一样的。
+ [关于this问题的例子](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout#%E5%85%B3%E4%BA%8Ethis%E7%9A%84%E9%97%AE%E9%A2%98)


> 更多资料参考：[MDN:setTimeout](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/setTimeout)

------
#### 1.7.3.4 如何用setTimeout实现setInterval?
> 递归
```js
function setInter(fn,ms){
  let timeOut = (fn,ms)=>{
    setTimeout(()=>{
      fn();
      timeOut(fn,ms);
    },ms)
  };
  timeOut(fn,ms)
};
setInter(()=>{console.log('hello world!')},2000);
```
>详细资料可以参考：[《setInterval 有什么缺点？》](https://zhuanlan.zhihu.com/p/51995737)

------

### 1.7.2 Promise

#### 1.7.1.1 为什么需要Promise？

+ 在早期的Javascript中，只支持定义回调函数来表明异步操作完成。串联多个异步操作，通常需要深度嵌套回调函数，又称”回调地狱“。Promise规范的出现，解决了回调地狱的问题，同时使代码更具有可读性和维护性。
------

#### 1.7.1.2 Promise 的基本用法

+ 1. Promise对象的两个特点：
	- 对象的状态不受外界的影响。Promise对象代表一个异步操作，有3中状态：Pending（进行中），Fulfilled(已成功)和Rejected（已失败）。只有异步操作的结果可以决定当前是哪一种状态。
	- 一旦状态改变就不会再变。Promise对象的状态改变只有两种可能：从Pending变为Fulfilled和从Pending变为Rejected。状态改变后就称为Resolved（已定型）。

+ 2. Promise构造函数
	- Promise对象是一个构造函数。用来生成Promise实例。Promise接收一个函数作为参数，这个函数也接收两个函数作为参数，分别是resolve和reject，它们是由js引擎提供的。
	
	- Promise实例生成以后，可以用then方法分别指定Resolve的状态和Rejected状态的回调函数。then方法可以接收两个回调函数作为参数。第一个回调函数是Promise对象的状态变为Resolved状态时调用，第二个回调函数是Promise对象的状态变为Rejected时调用。then方法返回的是一个新的Promise实例。
	
	- 如果调用resolve和reject 函数时带有参数，那么这些参数会被传递给回调函数。reject 函数的参数通常是Error实例，表示抛出的错误。resolve函数的参数除了正常的值以外，还可能是另一个Promise的实例。
	
```js
var p1 = new Promise(function(resove,reject){
	//...
});
var p2 = new Promise(function(resolve,reject){
	//...
	resolve(p1);
});
// p2的resolve方法将p1作为参数，p1的状态决定了p2的状态。p1的状态一旦改变，p2的状态也会随之改变，并立即执行p2的回调函数。
```

+ 3. **then()**
	- 定义于`Promise.prototype.then()`。
	- then方法返回一个新的Promise实例。
	- 采用链式的then可以指定一组按照次序调用的回调函数，而后一个回调函数会等待该Promise对象的状态发生变化，再别调用。

+ 4. **catch()**
	- 定义于`Promise.prototype.catch()`.
	- 相当于**.then(null,rejection)**。用于指定发生错误时的回调函数。
	- Promise在resolve语句后面再抛出错误，并不会捕获。因为Promise的状态已一旦改变，就会永久改变状态。
	- 一般来说，不要在then方法的第二个参数定义Rejected状态的回调参数，而应总是使用catch方法。
	- catch方法返回的还是一个Promise对象，因此在后面还可以继续跟then方法。
	
+ 5.**Promise.all()**
	- `Promise.all()`用于将多个Promise实例包装成一个新的Promise的实例。
	- `Promise.all()`方法接收一个数组作为参数，数组的每一项都是Promise实例，如果不是，就先到用Promise.resolve方法将参数转换为Promise的实例，再进一步处理。
```js
var promises = [1,2,3,4,5,6].map(function(id){
	return getJSON('/POST/'+id+".json");
});
Promise.all(promises).then(function(posts){
	//...
}).catch(function(error){
	//...
});
// 只有当6个实例的状态都变成fullfilled或者其中有一个变为rejected时，才会调用Promise.all方法后面的回调函数。
```

+ 6. **Promise.race()**
	- `Promise.race()`方法同样是将多个Promise的实例包装成一个新的Promise实例。
	- 只要参数中的一个promise实例率先改变状态，`Promise.race()`方法返回的promise对象的状态就会跟着改变。

+ 7. **Promise.resolve()**
	- `Promise.resolve()`可以将现有对象转换为Promise对象。
	- 如果参数是一个**Promise**实例，那么Promise.resolve将不做任何修改，原封不动地返回这个实例。
	- 如果参数是一个**thenable**对象，那么Promise.resolve将会把这个对象转换为Promise对象，然后立即执行thenable对象的then方法。
	- 如果参数是一个原始值，或是一个不具有then方法的对象，那么Promise.resolve方法将会返回一个新的Promise对象，状态为resolved。
	- 如果不带任何参数，直接返回一个Resolved状态的Promise对象。

+ 8. **Promise.reject()**
	- `Promise.reject()`方法也会返回一个新的Promise实例，状态为rejected。
	- `Promise.reject()`的参数会原封不动地作为reject的理由变成后续方法的参数！！！

------

### 1.7.3 async 函数

#### 1.7.3.1 async/await 的用法。

+ ES8 引入了`async/await`函数。可以使同步方式写的代码异步执行。

+ `async`函数返回一个promise对象，可以使用then方法添加回调函数。await是一个操作符，可以暂停异步函数代码的执行，等待一个Promise对象产生结果。如果await后面不是一个Promise对象，那它会被立即转换成一个resolve的Promise对象。

+ `async`函数内部抛出错误会导致返回的Promise对象变成rejected状态。

+ `await`关键字必须在声明了async的异步函数中使用。

+ 异步`async`函数如果使用reuturn关键字返回了值，这个值会被Proimise.resolve()包装成一个Promise对象。

+ `await`命令后面的Promise对象的运行结果可能是rejected，所以最好把await命令放在try...catch代码块中。

  ```js
  async function main(){
    try{
      var val1 = await firstStep();
      var val2 = await secondStep();
      var val3 = await thirdStep();
      console.log('Final:',val3);  
    }
    catch(err){
      console.log(err);
    }
  }
  ```
------

#### 1.7.3.2 async/await 函数里的宏任务与微任务

+ 由于async/await函数是promise的语法糖，所以类似于promise的宏任务微任务分发机制。async函数本身调用时相当于new Promise(),await 紧跟的语句相当于new Promise(args)里面的参数函数，这里作同步任务处理。注意：await 下一行如果还有同级的“字面同步”代码，那么这些代码同then()里面的参数函数，作为“微任务”处理。

------



### 1.7.4 同步异步与事件循环

#### 1.7.4.1 宏任务微任务与事件循环


- 宏任务：macro-task。包括：script(整体代码)，setInterval,setTimeout,setImmediate,I/O,UI rendering。
- 微任务：micro-task。包括：process.nextTick,Promise(then,catch,finally还有async函数里面的await之后下一行开始的代码。）。
- 事件循环的顺序：
	+ 事件循环从宏任务开始，即从script整体代码开始第一次循环。
	+ 全局上下文进入函数调用栈，然后同步任务按调用顺序依次进入，同步任务进入主线程，异步任务进入分线程，定时器/事件等被浏览器的**对应模块（线程）**执行（事件处理模块，定时器模块等），直到调用栈被清空（只剩全局，全局上下文永远在栈底）即同步任务执行完。
	+ 然后执行任务队列中 micro-task微任务。
	+ 当所有可以执行的micro-task执行完后，循环再次从macro-task开始，找到其中一个任务队列执行完毕，然后再执行所有的micro-task，这样一直循环下去。
	+ 其中每一个任务的执行，无论是macro-task还是micro-task，都是借助函数调用栈来完成。即都是主线程的同步任务执行完后，任务队列的任务进入主线程执行，即进入函数调用栈。

- 总结：主线程上代码的执行依托于执行栈（一个存储函数调用的栈结构，遵循先进后出）的原则），而事件循环机制的三个核心是宏任务、微任务和执行栈。js代码执行开始后，从宏任务（一般是script代码）开始，先把全局上下文放入执行栈，然后依次是当前同步代码。然后在执行宏任务过程中产生的微任务，（微任务队列依次放入执行栈）微任务执行完，这轮循环结束，开始下一轮循环。每当一个宏任务队列执行完后，就会去检查这个宏任务队列是否产生了微任务队列，如果有，马上会执行这个微任务队列。完了之后再去执行下一个宏任务队列。
	
------

#### 1.7.4.2 线程与进程

+ 线程和进程
	- **进程**是CPU资源分配的最小单位，**线程**是CPU调度的最小单位。进程好比工厂，有单独的专属自己的工厂资源。线程好比工人。一个进程由一个或多个线程组成，线程是一个进程中代码的不同执行路线。一个进程的内存空间是共享的，每个线程都可以用这些共享内存。
------

+ 多进程与多线程
	- **多进程**：同一个时间里，同一个计算机系统中允许两个或两个以上的进程处于运行状态。多进程带来的好处是明显的，比如可以听歌的同时，打开编辑器桥代码。编辑器和听歌软件的进程之间丝毫不会互相干扰。
	- **多线程**：程序中包含多个执行流，即在一个程序中可以同时运行多个不同的线程来执行不同的任务，也就是说允许单个程序创建多个并行执行的线程来完成各自的任务。
> 以Chrome浏览器中为例，当你打开一个 Tab 页时，其实就是创建了一个进程，一个进程中可以有多个线程（下文会详细介绍），比如渲染线程、JS 引擎线程、HTTP 请求线程等等。当你发起一个请求时，其实就是创建了一个线程，当请求结束后，该线程可能就会被销毁。
>
------
+ 如何理解js是单线程的，但又可实现异步编程？
	- js是单线程的，指的是依托于js这门语言在执行的任务在一条线上 ，一个任务执行完了，再能执行下一个任务。对于js来说，不是异步的。
	- 但是js的宿主环境，比如浏览器是多线程的，浏览器会通过事件驱动的方式，让JS能够异步执行，从而达到单线程进行异步执行的效果。
------

#### 1.7.4.3 同步与异步

+ 同步与异步
  - 同步，指的是当一个进程在执行某个请求的时候，如果这个请求需要等待一段时间才能返回，那么这个进程会一直等待下去，直到消息返回为止再继续向下执行。
  - 指的是当一个进程在执行某个请求的时候，如果这个请求需要等待一段时间才能返回，这个时候进程会继续往下执行，不会阻塞等待消息的返回，当消息返回时系统再通知进程进行处理。

+ 同步任务与异步任务
	- 同步任务：主线程上的任务排队执行。
	- 异步任务：不进入主线程，进入一个“任务队列”，当主线程中的任务执行完，才会从任务队列中取出异步任务放入主线程中执行。JS有三种异步任务：①鼠标键盘事件触发，onclick，onkeydown等；②网络事件触发，onload、onerror等；③定时器，setTimeout，setInterval。
	- 同步任务的概念依赖于某个任务的最后执行阶段是否在时间刻度上已经进入主线程中。

> HTML5提出Web Worker标准，允许JS创建出多个线程，但是创建出来的所有子线程全部受制于主线程，这样实现了同步和异步的任务。
>
> + JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。为了利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程，但是子线程完全受主线程控制，且不得操作DOM。所以，这个新标准并没有改变JavaScript单线程的本质。
> + javascript是单线程。单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。于是就有一个概念——任务队列。如果排队是因为计算量大，CPU忙不过来，倒也算了，但是很多时候CPU是闲着的，因为IO设备（输入输出设备）很慢（比如Ajax操作从网络读取数据），不得不等着结果出来，再往下执行。于是JavaScript语言的设计者意识到，这时主线程完全可以不管IO设备，挂起处于等待中的任务，先运行排在后面的任务。等到IO设备返回了结果，再回过头，把挂起的任务继续执行下去。
> + 所有任务可以分成两种，一种是**同步任务**（synchronous），另一种是**异步任务**（asynchronous）。同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；异步任务指的是，不进入主线程、而进入"任务队列"（task queue）的任务，只有等主线程任务执行完毕，"任务队列"开始通知主线程，请求执行任务，该任务才会进入主线程执行。
------
+ [面试题1] JS为什么需要异步执行机制？
	- 因为js里面的网络请求，定时器事件以及事件监听等事件，会消耗大量的时间，如果JS进行单线程执行，如果这些操作一直在浪费时间，就会导致整个页面无法往下执行，页面假死等情况，所以针对此类情况，浏览器为这些耗时较长时间的任务另外开辟了新的线程，所以这些任务都是**异步**的。
-------
+ [面试题2] 解释一下事件循环Event Loop？
  - JS是单线程的，主要的任务是处理用户的交互，而用户的交互无非就是响应DOM的增删改，使用事件队列的形式，一次事件循环只处理一个事件响应，使得脚本执行相对连续，所以有了事件队列，用来存储待执行的事件，那么事件队列的事件从哪里被push进来呢？那就是另外一个叫**事件触发的线程**做的事情了，它的主要作用是将在定时触发器线程、异步HTTP请求线程满足特定条件下的回调函数push到事件队列中，等待js引擎空闲的时候去执行，当然js引擎执行过程中有优先级之分，js代码执行开始后，从宏任务（一般是script代码）开始，先把全局上下文放入执行栈，然后依次是当前同步代码。然后在执行宏任务过程中产生的微任务，（微任务队列依次放入执行栈）微任务执行完，这轮循环结束，开始下一轮循环。每当一个宏任务队列执行完后，就会去检查这个宏任务队列是否产生了微任务队列，如果有，马上会执行这个微任务队列。完了之后再去执行下一个宏任务队列。

------
> 详见：[这波能反杀：深入核心，详解事件循环机制](https://segmentfault.com/a/1190000012646373)、[浏览器与Node的事件循环有何区别?](https://github.com/ljianshu/Blog/issues/54)、[事件循环，队列任务，宏微任务](https://www.yuque.com/woowwu/msyqpd/ypfh8g)
------

+ #### 1.7.4.1 [案例1] 判断下面代码的执行顺序。
```js
async function async1(){
	console.log('async1 start');
	await async2(); 					 // 相当于new Promise(async2())
	console.log('async1 end'); // 相当于promise实例后的then方法
};
async function async2(){
  // async2里面没有异步代码
	console.log('async2')
};
console.log('script start');
setTimeout(function(){
	console.log('setTimeout');
},0);
async1();
new Promise(function(resolve){
	console.log('promise');
	resolve();
}).then(function(){
	console.log('promise2')
});
console.log('script end');

// script start
// async1 start
// async2
// promise
// script end
// async1 end
// promise2
// setTime out
```

+ #### 1.7.4.2 [案例2] 判断下列代码执行顺序
```js
console.log('glob1');
setTimeout(function(){
  console.log('timeout1');
  process.nextTick(function(){
    console.log('timeout1_nextTick');
  });
  new Promise(function(resolve){
    console.log('timeout1_promise');
    resolve();
  }).then(function(){
    console.log('timeout1_then');
  });
});
// setImmediate方法可能不会被批准成为标准，目前只有最新版本的 Internet Explorer 和Node.js 0.10+实现了该方法
setImmediate(function(){
  console.log('immediate1');
  process.nextTick(function(){
    console.log('immediate1_nextTick');
  });
  new Promise(function(resolve){
    console.log('immediate1_promise');
    resolve();
  }).then(function(){
    console.log('immediate1_then');
  });
});

process.nextTick(function() {
    console.log('glob1_nextTick');
});

new Promise(function(resolve) {
    console.log('glob1_promise');
    resolve();
}).then(function() {
    console.log('glob1_then')
});

setTimeout(function() {
    console.log('timeout2');
    process.nextTick(function() {
        console.log('timeout2_nextTick');
    })
    new Promise(function(resolve) {
        console.log('timeout2_promise');
        resolve();
    }).then(function() {
        console.log('timeout2_then')
    })
});

process.nextTick(function() {
    console.log('glob2_nextTick');
});

new Promise(function(resolve) {
    console.log('glob2_promise');
    resolve();
}).then(function() {
    console.log('glob2_then')
});

setImmediate(function() {
    console.log('immediate2');
    process.nextTick(function() {
        console.log('immediate2_nextTick');
    })
    new Promise(function(resolve) {
        console.log('immediate2_promise');
        resolve();
    }).then(function() {
        console.log('immediate2_then')
    });
});

// 在node环境中测试！

// glob1
// glob1_promise
// glob2_promise

// glob1_nextTick
// glob2_nextTick
// glob1_then
// glob2_then

// -----下一轮----
// timeout1                    // 宏队列1的宏任务1
// timeout1_promise
// timeout1_nextTick					 // 宏队列1的宏任务1的微任务
// timeout1_then

// timeout2										 // 宏队列1的宏任务2
// timeout2_promise						
// timeout2_nextTick					 // 宏队列1的宏任务2的微任务
// timeout2_then

// immediate1									 // 宏队列2的宏任务1
// immediate1_promise
// immediate1_nextTick				 // 宏队列2的宏任务1的微任务
// immediate1_then

// immediate2
// immediate2_promise
// immediate2_nextTick
// immediate2_then
```

### 1.7.5 节流和防抖

#### 1.7.5.1 函数节流（throttle)
+ 函数节流指的是：一个函数执行一次后，只有大于设定的周期才会执行第二次。
+ 函数节流是为了限制函数触发的频次。即减小单位时间内事件触发的频次。**目标事件的完成是能很清楚地被计算机界定的，只是这个事件被高频触发了**。
+ ✅**节流使用场景**：<u>鼠标不断点击</u>，mousedown事件；鼠标滚动事件，比如是否滑到底部自动加载更多。
+ 代码实现：定义一个节流throttle函数，这个函数接收两个参数，一个是原始的函数，第二个是节流时间，返回一个新的函数。返回出来的新函数，不能丢失原函数的this指向，给原函数传递的参数也不能改变。

#### 1.7.5.2 函数节流的实现
```html
<body>
  <input type="text">
  <input type="submit" id="input">
</body>

<script>
  let btn = document.getElementById('input');
  let tSubmit = throttle(submit, 2000) // 页面初始化后throttle函数即刻执行
  btn.addEventListener('click', tSubmit);
  function submit() {
    console.log(1);
  }
  // 【节流函数】
  function throttle(fun, delay) {
    // begin 一开始表示页面初始化后的即刻时间，如果后续点击即刻执行新函数，那么begin刷新，此时begin表示上次fun函数的执行时间刻度
    var t = null, begin = new Date().getTime();
    return function () {
      // curT 表示节流后返回的新函数“执行”的时间刻度
      let that = this, args = arguments, curT = new Date().getTime();
      clearTimeout(t);
      if (curT - begin >= delay) {
        fun.apply(that, args);
        // 初始化时间重新赋值为“点击事件发生后即刻就执行了fun函数的时间”
        begin = curT;
      } else {
        t = setTimeout(function () {
          fun.apply(that, args);
        }, delay)
      }
    }
  }
</script>

```
------

#### 1.7.5.3 函数防抖(debounce)
+ 函数防抖指的是：一个需要频繁触发的函数，在规定时间内只让**最后一次**生效，前面的不生效。
+ 函数防抖，针对最后一次触发事件后，在限制时间内，有足够的时间使其成为最后一次的事件触发。**目标事件的完成时机不能清楚地被计算机界定，目标事件的完成时机取决于用户不确定的高频操作何时有目的地停止**。
+ ✅**防抖使用场景**：<u>search搜索联想</u>，用户不断输入值，用防抖来节约请求资源；window触发size事件时，不断的调整窗口大小会不断地触发这个事件，用防抖来让其只触发一次。
+ 代码实现：定义一个debounce函数，这个函数接收两个参数，一个是初始函数，第二个是防抖时间。返回一个新函数，返回出来的新函数，不能丢失原函数的this指向；给原函数传递的参数也不能改变。

#### 1.7.5.4 函数防抖的实现

```html
<body>
  <input type="text" id="text">
</body>

<script>
  let btn = document.getElementById('text');
  let debSearch = debounce(search, 2000, false)
  btn.addEventListener('keyup', Search);

  function search(e) {
    console.log(this.type + '文本框，正在搜索' + e.target.value);
  }
	// 【防抖实现】
  function debounce(fun, time, triggerFirst) {
    let t = null;
    return function () {
      let that = this, args = arguments;
      clearTimeout(t);
      // 注意 clearTimeout(t) 之后，只是清除了计时器（要执行的回调），但是计时器的ID还依然存在
      // triggerFirst true表示初次事件触发立即执行；false表示初次事件触发延迟执行
      if (triggerFirst) {
        // t===null 表示在防抖间隔时间之外，fun函数还未执行，需要立即执行它
        let unExec = !t;
        t = setTimeout(function () {
          t = null;// 只要在time间隔时间内，计时器的id都是存在的
        }, time);
        if (unExec) {
          fun.apply(that, args);
        };
      } else {
        t = setTimeout(function () {
          fun.apply(that, args);
        }, time)
      }
    }
  }
</script>
```
------

> 详细参考：[函数节流和防抖](https://github.com/ljianshu/Blog/issues/43) 、[函数防抖与节流](https://www.bilibili.com/video/BV1kk4y1B76Z?p=2)、 [跟着underscore学防抖](https://github.com/mqyqingfeng/Blog/issues/22)、[跟着underscore学节流](https://github.com/mqyqingfeng/Blog/issues/26)

------


## 1.8 代理与反射

### 1.8.1 什么是Proxy？

+ Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”，即对编程语言进行编程。Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

+ 代理是使用Proxy构造函数创建的，这个构造函数接收两个参数：目标对象和处理程序对象。

```js
  const target = {
    id:'target';
  }
  const handler = {};
  const proxy = new Proxy(target,handler);
```
+ 使用代理的主要目的是可以定义捕获器。捕获器是在处理程序对象中定义的“基本操作的拦截器”。每次在代理对象上调用这些基本操作时，代理可以在这些操作传播到目标对象之前先调用捕获器函数，从而拦截并修改相应的行为。

------

### 1.8.2 Reflect对象创建的目的？
- 1.将 Object 对象的一些明显属于语言内部的方法（比如 Object.defineProperty，放到 Reflect 对象上。
- 2.修改某些 Object 方法的返回结果，让其变得更合理。
- 3.让 Object 操作都变成函数行为。
- 4.Reflect 对象的方法与 Proxy 对象的方法一一对应，只要是 Proxy 对象的方法，就能在 Reflect 对象上找到对应的方法。这就让 Proxy 对象可以方便地调用对应的 Reflect 方法，完成默认行为，作为修改行为的基础。也就是说，不管 Proxy 怎么修改默认行为，你总可以在 Reflect 上获取默认行为。
------
## 1.9 js语言特性与应用技巧

### 1.9.1 如何编写高性能的 Javascript ？

- 1.使用位运算代替一些简单的四则运算。
- 2.避免使用过深的嵌套循环。
- 3.不要使用未定义的变量。
- 4.当需要多次访问数组长度时，可以用变量保存起来，避免每次都会去进行属性查找。

详细资料可以参考：[《如何编写高性能的 Javascript？》](https://zhuanlan.zhihu.com/p/34780474)

------

### 1.9.2 js 延迟加载的方式有哪些？

- js 的加载、解析和执行会阻塞页面的渲染过程，因此我们希望 js 脚本能够尽可能的延迟加载，提高页面的渲染速度。我了解到的几种方式是：

  - 第一种方式是我们一般采用的是将 js 脚本放在**文档的底部**，来使 js 脚本尽可能的在最后来加载执行。

  - 第二种方式是给 js 脚本添加 **defer** 属性，这个属性会让脚本的加载与文档的解析同步解析，然后在文档解析完成后再执行这个脚本文件，这样的话就能使页面的渲染不被阻塞。多个设置了 defer 属性的脚本按规范来说最后是顺序执行的，但是在一些浏览器中可能不是这样。

  - 第三种方式是给 js 脚本添加 **async** 属性，这个属性会使脚本异步加载，不会阻塞页面的解析过程，但是当脚本加载完成后立即执行 js 脚本，这个时候如果文档没有解析完成的话同样会阻塞。多个 async 属性的脚本的执行顺序是不可预测的，一般不会按照代码的顺序依次执行。

  - 第四种方式是**动态创建 DOM 标签**的方式，我们可以对文档的加载事件进行监听，当文档加载完成后再动态的创建 script 标签来引入 js 脚本。

> 详细资料可以参考：[《JS 延迟加载的几种方式》](https://blog.csdn.net/meijory/article/details/76389762)、[《HTML 5 `<script>` `async` 属性》](http://www.w3school.com.cn/html5/att_script_async.asp)

### 1.9.3 把 script 标签放在页面的最底部的 body 封闭之前和封闭之后有什么区别？浏览器会如何解析它们？

详细资料可以参考：
[《为什么把 script 标签放在 body 结束标签之后 html 结束标签之前？》](https://www.zhihu.com/question/20027966)
[《从 Chrome 源码看浏览器如何加载资源》](https://zhuanlan.zhihu.com/p/30558018)

------
### 1.9.4 JS中的命名规则

+ 第一个字符必须是字母、下划线（_）或美元符号（$）
+ 余下的字符可以是下划线、美元符号或任何字母或数字字符
+ 一般我们推荐使用驼峰法来对变量名进行命名，因为这样可以与 ECMAScript 内置的函数和对象命名格式保持一致。
-------
### 1.9.5 JS的严格模式有哪些限制？
+ 变量必须先声明后再使用。
+ 函数的参数不能有同名属性，否则报错。
+ 不能使用with语句
+ 禁止this指向全局对象。
------
# 2. BOM

+ BOM(Browser Object Model) 是浏览器对象模型。BOM提供了与网页无关的浏览器功能对象。

## 2.1 window 对象
+ BOM的核心是window对象。即窗口对象。

### 2.1.1 窗口位置
> 高程4，P362页
+ 窗口对象：现代浏览器提供了**screenLeft**和**screenTop**属性，用于表示窗口相对于屏幕左侧和顶部的位置，返回值的单位是CSS像素。
+ 可以使用moveTo()和moveBy()方法移动窗口。这两个方法接收两个参数，其中moveTo()接收要移动到的新位置的绝对坐标x和y,而moveBy()则接收相对当前位置在两个方向上移动的像素数。

### 2.1.2 窗口大小
> 高程4，P363页
+ 现代浏览器都支持4个属性。**outerWidth**和**outerHeight**返回浏览器窗口自身的大小。**innerWidth**和**innerHeight**返回浏览器窗口中页面视口的大小（不包含浏览器边框和工具栏）。
+ 可以使用**resizeTo()**和**resizeBy()**方法调整窗口的大小。这两个方法都接收两个参数，resizeTo()接收新的宽度和高度值，而resizeBy()接收宽度和高度各要缩放多少。

### 2.1.3 视口位置
> 高程4，P364页。
+ 浏览器窗口尺寸通常无法满足完整显示整个页面。度量文档相对于视口滚动距离的属性有两对，返回相等的值。**window.pageXoffset/window.scrollX**和**window.pageYoffset/window.scrollY**。
+ 可以使用**scroll()**、**scrollTo()**和**scrollBy()**方法滚动页面。这三个方法都接收表示相对视口距离的x和y坐标，这两个参数在前两个方法中表示要滚动到的新坐标，在最后一个方法中表示要滚动的距离。

### 2.1.4 导航与打开新窗口
> 高程4，P365页。
+ **window.open()**方法可以用于导航到指定的URL,也可以用于打开新的浏览器窗口。这个方法接收4个参数：要加载的URL、目标窗口、特性字符串和表示新窗口在浏览器历史记录中是否替代当前加载页面的布尔值。如果window.open()方法的第二个参数是一个已经存在的窗口或窗格(frame)的名字，则会在对应的窗口或窗格中打开URL。

## 2.2 location 对象

+ **location**对象是最有用的BOM对象之一，提供了当前窗口中加载文档的信息，以及通常的导航功能，这个对象的独特之处在于它既是window对象的属性，有时document对象的属性。

### 2.2.1 location对象的属性
+ 假设浏览器当前加载的URL是 http://foouser:barpassword@www.wrox.com:80/WileyCDA/?q=javascript#contents ， location 对象的内容如下标所示。
+ 高程4，P372页。

### 2.2.2 通过location对象修改浏览器的地址
+ 1. 最常见的是使用**assign()**方法并传入一个URL. `location.assign('http://www.wrox.com');`,这行代码会立即启动导航到新的URL的操作，同时在浏览器历史记录中增加一条记录。注意此时修改的是浏览器当前加载的页面。
> 除了location.hash之外，只要修改location的一个属性，就会导致页面重新加载新的URL。

+ 2. 如果不希望导航到新的页面后增加历史记录，可以使用**replace()**方法。这个方法接收一个URL参数，但重新加载后不会增加历史记录。等页面重定向到新页面后，“后退”按钮是禁用状态。
```js
setTimeout(()=>location.replace("http://www.wrox.com/"),1000)
```
+ 3. 最后一个修改地址的方法是**reload()**。它能重新加载当前显示的页面。调用reload()而不传参数，页面会以最有效的方式重新加载。也就是说，如果页面上次请求以来没有修改过，浏览器可能会从缓存中加载页面。如果想强制从服务器重新加载，可以给reload()传true参数。
```js
location.reload();
location.reload(true);
```

## 2.3 navigator对象

+ 客户端通过navigator对象识别浏览器。navigator对象上定义了一系列的属性和方法，访问navigator.userAgent, 可以返回浏览器的用户代理字符串，这个属性所有浏览器都支持。

## 2.4 screen对象

+ screen对象保存了客户端显示器的信息。

## 2.5 history对象

+ history对象表示当前窗口首次使用以来用户的导航历史记录。因为history是window的属性，所以每个window都有history对象。

+ **history.go()**方法可以在用户历史记录中沿任何方向导航，可以前进也可以后退。这个方法只接受一个参数，这个参数可以是一个整数，表示前进或后退多少步。负值表示在历史记录中后退，正值表示在历史记录中前进。
```js
history.go(-1);//后退一页
history.go(1);//前进一页
history.go(0);//刷新当前页面
```

+ **history.back()**和**history.foward()**模拟了了浏览器的后退和前进按钮。分别表示前进一页和后退一页。
+ **history**对象还有一个length属性，表示历史记录中有多少个条目。

+ HTML5为history对象添加了方便的**状态管理特性**。hashchange 会在页面URL散变化时触发，开发者可以在此时执行某些操作。而状态管理API则可以让开发者改变浏览器URL而不会重新加载页面。为此，可以使用**history.pushState()**方法。这个方法接收三个参数：一个state对象，一个新状态的标题和一个可选的相对URL。pushState()方法执行后，状态信息就会被推送到历史记录中，浏览器地址栏也会改变以反映新的相对URL。
```js
let stateObject = {foo:'bar'};
history.pushState(stateObject,"my title",'bar.html');
```
> 详细资料参考高程4，P381页
------
------
# 3. DOM

+ DOM(Document Object Model) 是HTML和XML文档的编程接口。DOM表示由多层节点构成的文档。
------
## 3.1 DOM基础
### 3.1.1 节点层级
+ 任何HTML文档或XML文档都可以用DOM表示为一个由节点构成的层级结构。document节点表示每个文档的根节点，根节点的唯一子节点是<html>元素（文档元素），即docuemntElement节点。文档元素是最外层的元素，所有其他元素都存在于这个元素之内，每个文档都只能有一个文档元素。
------
#### 3.1.1.1 节点类型（熟悉前三种）
+ 每个节点都有**nodeType**属性，表示该节点的类型。节点类型由定义在Node类型上的12个数值常量表示。
+ 前三种是最为常见的节点类型。分别是：
	- **元素节点**：Node.ELEMENT_NODE(1)  
	- **属性节点**：Node.ATTRIBUTE_NODE(2)
	- **文本节点**：Node.TEXT_NODE(3)
------
#### 3.1.1.2 三个表示【节点信息】的属性
+ 1. 每个节点都有**nodeType**属性，表示该节点的类型。
+ 2. **nodeName**，保存着节点的名称信息。
+ 3. **nodeValue**，保存着节点的值。
> 对于元素节点，nodeName 保存元素的标签名，nodeValue的值为null。
------
#### 3.1.1.3 八个表示【节点关系】的属性
+ 1. **childNodes**属性或**children**属性，其值是一个类数组的NodeList的实例对象，使用中括号可以访问NodeList 中的元素。
+ 2. **parentNode**属性，指向其DOM树中的父元素。
+ 3. **previousSibling**属性，同级节点的上一个节点。
+ 4. **nextSibling**属性，同级节点的下一个节点。
+ 5. **firstChild**属性，指向childNode中的第一个子节点。
+ 6. **lastChild**属性，指向childNode中的最后一个子节点。
+ 7. **hasChildNodes()**方法，这个方法如果返回true,则说明节点有一个或多个子节点。
+ 8. **contains()**方法，用来判断一个元素是不是另一个元素的后代。contains()方法应该在要搜索的祖先元素上调用，参数是待确定的目标节点。
```js
console.log(document.documentElement.contains(document.body));//true
```
> **children**属性和**childNodes**属性：如果元素的子节点类型全部是元素类型，那么children和childNodes中包含的节点是一样的。

------
#### 3.1.1.4 六个【操作节点】的方法
+ 1. **appendChild()**,用于在childNodes列表末尾**添加**节点，返回新添加的节点。
+ 2. **insertBefore()**,接收两个参数：要**插入**的节点和参照节点。用于把节点放到childNodes中的特定位置而不是末尾。
+ 3. **replaceChild()**,接收两个参数：要插入的节点和要替换的节点。要**替换**的节点会被返回，并从文档树中完全移除，要插入的节点取而代之。
+ 4. **removeChild()**, 接收一个参数，即要被**移除**的节点，被移除的节点会被返回。
+ 5. **cloneNode()**,接收一个布尔值参数，表示是否执行深复制。用于创建调用这个方法的节点的一个完全相同的副本。
+ 6. **normalize()**，处理文档子树中的文本节点。由于解析器实现的差异或DOM操作等原因，可能会出现并不包含文本的文本节点，或者文本节点之间互为同胞关系。在节点上调用normalize()方法，会检测这个节点的所有子节点，从中搜索上述两种情形，如果发现空文本节点，则将其删除；如果两个同胞节点相邻，则将其合并为一个文本节点。
------
### 3.1.2 元素类型
+ Element类型是最常见的类型。表示XML或HTML的元素。
+ nodeType值为1，nodeName值为元素的标签名，nodeValue的值为null。
------
#### 3.1.2.1 元素的标准属性
+ 1. **id**, 元素在文档中的唯一标识符。
+ 2. **title**, 包含元素的额外信息，通常以提示条形式展示。
+ 3. **lang**, 元素内容的语言代码。
+ 4. **dir**, 语言的书写方向（”ltr"表示左到右，“rtl"表示右到左)。
+ 5. **className**, 相当于class属性，用于指定元素的CSS类。
------
#### 3.1.2.2 与元素的属性相关的方法
+ 1. **getAttribute()**,接收属性名，获得其属性值。注意，传给getAttribute()的属性名与实际的属性名是一致的。
```js
let div = document.getElementById('myDiv');
console.log(div.getAttribute('class'));//bd
```
+ 2. **setAttribute()**,接收两个参数：要设置的属性名和属性值。如果属性已存在，就会以指定值替换原来的值。
+ 3. **removeAttribute()**,接收属性名，用于从元素中删除属性。会把整个属性完全从元素中去掉。
------
#### 3.1.2.3 如何创建元素?
+ 使用**document.createElement()**方法创建新元素。接收一个参数，即要创建元素的标签名。
> 要把创建的元素添加到文档树，可以使用`appendChild()`、`insertBefore()`或`replaceChild()`。
------

### 3.1.3 属性类型
+ 技术层面，**属性**是存在于元素attributes属性中的节点。
+ nodeType值为2，nodeName值为属性名，nodeValue值为属性值。
+ 属性节点尽管是节点的一种，却不认为是DOM文档树的一部分。
+ 可以使用**document.createAttribute()**方法创建新的Attr节点，参数为属性名。
------
### 3.1.4 文本类型
+ Text节点由Text类型表示，包含按照字面解释的纯文本，也可能包含转义后的HTML字符，但不包含HTML代码。
+ nodeType值为3，nodeName值为”#text",nodeValue值为节点中所包含的文本。
+ 可以使用**document.createTextNode()**来创建新的文本节点，接收一个参数，即要插入节点的文本。
------
## 3.5 DOM编程

### 3.5.1 动态脚本
+ 两种方式通过<script>标签为网页添加脚本：引入外部文件和直接插入源代码。
> 注意，通过innerHTML属性创建的<script>元素永远不会执行。
### 3.5.2 动态样式
+ CSS样式在HTML中可以通过两个元素加载：<link>元素用于包含CSS外部文件，<style>元素用于添加嵌入样式。

------
### 3.5.3 获取元素的API
#### 3.5.3.1 只暴露在document对象上的API
+ ① **document.getElementById()**,接收一个参数，即要获取元素的ID，如果找到了就返回这个元素，没有找到就返回null。
+ ② **document.getElementsByTagName()**,接收一个参数，即要获取元素的标签名，返回一个NodeList实例。
#### 3.5.3.2 暴露在document对象和所有HTML元素实例上的API。
+ ③ **querySelector()**方法接收`CSS选择符参数`，返回匹配该模式的第一个后代元素，如果没有匹配项则返回null。
+ ④ **querySelectorAll()**方法接收`CSS选择符参数`，返回匹配到的所有节点组成的NodeList实例。
+ ⑤ **getElementsByClassName()**方法接收一个参数（类名），即包含一个或多个类名的字符串，返回类名中包含相应类的元素的NodeList实例。如果提供了多个类名，则顺序无关紧要。
------
## 3.6 HTML5
+ HTML5为标准的DOM提供了大量扩展。
------
### 3.6.1 自定义数据属性
+ HTML5允许给元素指定非标准的属性，但要使用前缀data-以便告诉浏览器，这些属性是自定义属性。
+ 定义了自定义属性后，可以使用dataset属性来访问。
+ 要注意在HTML中的下划线命名法，对应JS里面要使用驼峰命名来对应。
```html
<div id="myDiv" data-appId="1234" data-my-name="Nicholas"> </div>
<script>
	let div = document.getElementById('myDiv');
  let appId = div.dataset.appId;
  let myName = div.dataset.myName;
</script>
```
------
### 3.6.2 扩展DOM
#### 3.6.2.1 插入标记
+ HTML5标准：**innerHTML**和**outerHTML**。
	- 读取**innerHTML**属性，会返回元素所有后代的HTML字符串，包括元素、注释和文本节点，而写入innerHTML属性，则会根据提供的字符串值以新的DOM子树替代元素中原来包含的所有节点。
	- 读取**outerHTML**属性，会返回调用它的元素本身（及所有后代元素）的HTML字符串。
+ 未入选的标准：**innerText**和**outerText**
	- 读取**innerText**，会按照深度优先的顺序将子树中的所有文本节点的值拼接起来。在写入值时，innerText会移除元素的所有后代并插入一个包含该值的文本节点。
	- 读取**outerText**，会和**innerText**返回同样的内容。但在写入文本值时，outerText就大不相同了，写入文本值时，outerText会移除替换掉整个元素。
------

## 3.7 DOM2和DOM3
### 3.7.1 存取元素的样式
+ 任何支持style属性的HTML元素在JS中都会有一个对应的style属性。其中**只包含直接由HTML元素的 style属性**为元素设置的所有样式信息，但不包含通过层叠机制从文档样式或外部样式中继承来的样式。HTML style属性中的CSS属性在JS中的style对象中有对应的属性。因为在CSS中属性名使用连字符表示法，所以在JS中这些属性必须转换为驼峰大小写形式。
```js
let myDiv = document.getElementById('MyDiv');
myDiv.style.backgroundColor='red';
myDiv.style.width='100px';
myDiv.style.height='200px';
```
------
### 3.7.2 计算样式

+ style对象中包含支持style属性的元素为这个属性设置的样式信息，但不包含从其他样式表层叠继承的同样影响该元素的样式信息。
+ DOM2 在document.defaultView中增加了**getComputedStyle()**方法。这个方法接收两个参数：要取得计算样式的元素和伪元素的字符串(如：":after")。如果不需要查询伪元素，则第二个参数可以传null。getComputedStyle()方法返回一个CSSStyleDeclaration对象（同style属性的类型），包含元素的计算样式。
+ 关于计算样式，在所有的浏览器中计算样式都是只读的，不能修改getComputedStyle()方法返回的对象。
> 更多内容参考：高程4，P469页。
------

### 3.7.3 元素尺寸
#### 3.7.3.1 偏移尺寸
> 高程4. P473页
#### 3.7.3.2 客户端尺寸
> 高程4. P474页
#### 3.7.3.3 滚动尺寸
> 高程4. P475页
------

## 3.8 事件

+ Javascript与HTML的交互是通过事件实现的，事件代表文档或浏览器窗口中某个有意义的时刻。
+ **事件流**：事件流描述了页面接收事件的顺序。
------
### 3.8.1 三种事件流
+ **事件冒泡**：IE事件流被称为事件冒泡。这是因为事件被定义为从最具体的元素（文档树中最深的节点）开始触发，然后向上传播至没有那么具体的元素。
+ **事件捕获**：Netscape提出了事件捕获。事件捕获的意思是最不具体的节点应该先收到事件，而最具体的节点应该最后收到事件。
+ **DOM事件流**：DOM2规范规定，事件流分为3个阶段：事件捕获，到达目标和事件冒泡。
------
### 3.8.2 事件处理程序
#### 3.8.2.1 注册和移除事件处理程序的两种方式：
+ 1.  传统方式是把一个函数赋值给DOM元素的一个事件处理程序属性。比如`onclick`。通过把事件处理程序的属性值设置为null，就可以移除事件。
```js
// 注册
let btn = document.getElementById('myId');
btn.onclick=function(){
	console.log('Clicked');
}
//  移除
btn.onclick=null;
```

+ 2.  DOM2为事件处理程序定义了两个方法：**addEventListener()和removeEventListener()** 。这两个方法暴露在所有的DOM节点上，它们接收三个参数：事件名，事件处理函数和一个布尔值，true表示在捕获阶段调用事件处理程序，false(默认值)表示在冒泡阶段调用事件处理程序。
```js
let btn = document.getElementById('myBtn');
btn.addEventListener("click",()=>{
	console.log(this.id);
},false);
//---
let handler = function(){
	console.log(this.id);
};
btn.addEventListener('click',handler,false);
btn.removeEventListener('click',handler,false);
```
+ ✅比较两种添加事件处理程序的方式？
	- 1. `element.onclick`添加的事件处理程序只会在事件流的冒泡阶段被处理。而`addEventListener()`添加的事件处理程序可以通过定义第三个参数来决定在事件流的哪个阶段处理事件程序。
	- 2. 使用`addEventListener()`注册事件的主要优势是可以为同一个事件添加多个事件处理程序。
	- 3. 通过`addEventListener()`添加的事件处理程序只能使用`removeEventListener()`并传入与添加时同样的参数来移除。所以使用`addEventListener()`添加的匿名函数无法被移除。
------
#### 3.8.2.2 还有哪种事件处理程序的注册方式？
+ 通过直接在HTML元素的属性上注册事件处理程序
```html
<input type="button" value="Click Me" onclick="console.log(event,type)">
```
------
### 3.8.4 事件对象
+ DOM发生事件时，所有相关信息都会被收集并存储在一个名为`event`的对象中。
+ 事件对象包含与特定事件相关的属性和方法。不同的事件生成的事件对象也会包含不同的属性和方法。
------
#### 3.8.4.1 列举一些事件对象的公共属性和方法
+ 1. **currentTarget** ,表示事件处理程序所在的元素。
+ 2. **target** ,表示事件目标，它指向触发事件的最底层节点。
+ 3. **preventDefault()**方法，用于阻止特定事件的默认动作，可以用`return false;`来替代。
+ 4. **stopPropagation()**方法，用于立即阻止事件流在DOM结构中传播，取消后续的事件捕获或冒泡。
+ 5. **eventPhase**属性，用于确定该事件流当前所处的阶段。如果事件处理程序在捕获阶段被调用，`eventPhase`值为1；如果事件处理程序在目标上被调用，`eventPhase`的值为2；如果事件处理程序在冒泡阶段被调用，则`eventPhase`等于3。
------

### 3.8.5 事件类型
#### 3.8.5.1 用户界面事件
+ 1. **load**: 当”目标“完全加载后触发。”目标看“可以是window，也可以是普通元素，也可以是窗套<frameset>（当所有窗格<frame>都加载完后触发）。
+ 2. **unload**: 当”目标“完全卸载后触发。`upload`事件一般是在从一个页面导航到另一个页面时触发，最常用于清理引用，以避免内存泄露。
+ 3. **resize**: 浏览器窗口被缩放到新高度或宽度时，触发`resize`事件。这个事件在window上触发。
+ 4. **scroll**: 反映了页面中相应元素的变化。
------
#### 3.8.5.2 焦点事件
+ 1. **blur**: 当元素失去焦点时触发。这个事件不冒泡。
+ 2. **focus**: 当元素获得焦点时触发。这个事件不冒泡。
+ 3. **focusout**: 当元素失去焦点时触发。支持事件冒泡。
+ 4. **focusin**: 当元素获得焦点时触发。支持事件冒泡。
------
#### 3.8.5.3 鼠标事件
+ 1. **click**: 单击鼠标左键或按键盘回车键时触发。
+ 2. **dbclick**: 双击鼠标左键触发。
+ 3. **mousedown**: 用户按下任意鼠标键时触发。
+ 4. **mouseenter**: 把鼠标光标从元素外部移入元素内部时触发。不冒泡。不在光标经过后代元素时触发。
+ 5. **mouseleave**: 把鼠标光标从元素内部移到元素外部时触发。不冒泡。不在光标经过后代元素时触发。
+ 6. **mousemove**: 鼠标光标在元素上移动时反复触发。
+ 7. **mouseout**: 鼠标光标从一个元素移动到另一个元素时触发。移到的元素可以是原始元素的外部元素，也可以是原始元素的子元素。
+ 8. **mouseover**: 把鼠标光标从元素内部移入元素内部时触发。
+ 9. **mouseup**: 用户释放鼠标键时触发。

> mouseover 和 mouseenter 的区别？
> >当鼠标移动到元素上时就会触发 mouseenter 事件，类似 mouseover，它们两者之间的差别是 mouseenter 不会冒泡。由于 mouseenter 不支持事件冒泡，导致在一个元素的子元素上进入或离开的时候会触发其 mouseover 和 mouseout 事件，但是却不会触发 mouseenter 和 mouseleave 事件。
> >
> >详细资料可以参考：[《mouseenter 与 mouseover 为何这般纠缠不清？》](https://github.com/qianlongo/zepto-analysis/issues/1)
-------
#### 3.8.5.4 键盘与输入事件
+ 1. **keydown**: 用户按下键盘上某个键时触发，持续按住重复触发。
+ 2. **keypress**: 用户按下键盘上某个键并产生字符时触发，持续按住重复触发。ESC键也会触发。DOM3推荐使用`textInput`事件。
+ 3. **keyup**: 用户释放键盘上某个键时触发。
+ 4. **textInput**: 当字符被输入到可编辑区域时触发。textInput只有在新字符被插入时才会触发。
------
#### 3.8.5.5 HTML5事件
> 高程4 P523

------
### 3.8.6 事件委托
+ **事件委托**本质上是利用了浏览器事件冒泡的机制。因为事件在冒泡过程中会上传到父节点，并且父节点可以通过事件对象获取到目标节点，<u>因此可以把子节点的监听函数定义在父节点上，由父节点的监听函数统一处理多个子元素的事件，这种方式称为事件代理</u>。
+ 使用事件代理我们可以不必要为每一个子元素都绑定一个监听事件，这样减少了内存上的消耗。并且使用事件代理我们还可以实现事件的动态绑定，比如说新增了一个子节点，我们并不需要单独地为它添加一个监听事件，它所发生的事件会交给父元素中的监听函数来处理。
> 详细资料可以参考：[《JavaScript 事件委托详解》](https://zhuanlan.zhihu.com/p/26536815)
------

# 4. 网络请求

## 4.1 JSON

### 4.1.1 JSON语法

+ JSON 是一种通用的数据格式，基于文本，优于轻量，用于交换数据。它不是编程语言。JSON出现后，迅速成为Web服务的事实序列化标准。
+ JSON语法支持3种类型的值，简单值（包括字符串、数值、布尔值及null，不包括undefined)，对象和数组。
+ JSON字符串必须用双引号。
+ 与Javsscript对象字面量相比，JSON有两处不同。首先，它没有变量声明；其次，没有分号。JSON对象中的属性名必须要用双引号包围起来。
+ 对象和数组通常会作为JSON数组的顶级结构，以便创建大型复杂数据结构。
------
### 4.1.2 JSON对象

+ ES5增加了JSON全局对象，可以正式解析JSON。

#### 4.1.2.1 JSON序列化函数
+ **JSON.stringify()**可以把一个JS对象，序列化成一个JSON字符串。默认情况下，JSON.stringify()会输出不包含空格或缩进的JSON字符串。
```js
let book = {
	title:"Professinal Javascript",
	authors:[
		"Nicholas C. Zakas",
		"Matt Frisbie"
	],
	edition:4,
	year:2017
};
let jsonText = JSON.stringify(book);
console.log(jsonText);
//{"title":"Professional Javscript","author":["Nicholas C. Zakas","Matt Frisibie],"edition":4,"year":2017}}
```
+ **JSON.stringify()**还可以额外接收两个参数。第一个额外参数是个过滤器，可以是数组或函数；第二个额外参数是用于缩进结果JSON字符串的选项，如果这个参数是数值，表示每一级缩进的空格数，如果是字符串，那么JSON字符串中就会使用这个字符串而不是空格来表示缩进。单独或组合使用这些参数可以更好地控制JSON序列化。
------
#### 4.1.2.2 JSON解析函数
+ **JSON.parse()**函数可以接收一个JSON字符串，解析成响应的JS值。
+ **JSON.parse()**函数也可以额外接收一个参数。该参数是一个函数（还原函数）。还原函数接收两个参数，属性名和属性值，另外也需要返回值。
------

## 4.2 网络请求的前端实现
+ Ajax(Asynchronous Javascript+XML)即异步JS+XML技术。使用Ajax技术可以**在从服务器获取数据的同时不刷新页面**。
+ Ajax通信与数据格式无关。
+ 传统的Ajax技术指的就是通过XHR对象实现的不刷新页面获取数据的技术。
------
### 4.2.1 XHR对象
+ XHR（XMLHttpRequest）对象把Ajax推向了历史的舞台。
+ 所有现代浏览器都支持通过`XMLHttpRequest`构造函数原生支持XHR对象。
```js
let xhr = new XMLHttpRequest();
```
+ 使用<span id="xhrasync">XHR对象</span>，首先要调用**open()**方法，这个方法接收三个参数：请求类型（‘get’，‘post’等），请求URL，以及表示请求是否异步的布尔值。
+ **send()**方法接收一个参数，作为请求体发送的数据。如果不需要发送请求体，则必须传null。
+ 收到响应后，XHR对象的**responseText**属性会被填充上作为响应体返回的文本。**status**属性会被填充上响应的HTTP状态码。首先检查`status`属性以确保响应成功返回，一般来说HTTP状态码为2XX表示成功，此时`responseText`属性中会有内容。如果状态码是304，表示资源未被修改过，是从浏览器缓存中直接拿取的。
+ XHR对象有一个**readyState**属性，表示当前处在请求/响应过程的哪个阶段。当这个属性的值为4时，表示已经收到所有响应，可以使用了。每次`readyState`从一个值变为另一个值，都会触发**readyStateChange**事件，可以借此机会检查`readyState`的值。
+ 每个HTTP请求和响应都会携带一些头部字段。默认情况下，XHR请求会发送如下字段：`Cookie`,`Host`等。如果需要发送额外的请求头部，可以使用**setRequestHeader()**方法。这个方法接收两个参数：头部字段的名称和值。为保证请求头部字段的值被发送，必须在`open()`之后，`send()`之前调用`setRequestHeader()`。服务器通过读取自定义头部可以确定适当的操作。
+ 可以使用**getRequestHeader()**方法从XHR对象获取响应头部，只要传入响应对象的名称即可。
------
#### 4.2.1.1 GET请求
+ **GET**请求是最常用的请求方法，用于向服务器查询某些信息。必要时，需要在GET请求后面添加查询字符串参数。
+ 如下所示为一个设置了自定义头部（可选）的GET请求。
```js
let xhr = new XMLHttpRequest();
xhr.onreadystatechange = function(){
	if(xhr.readyState===4){
		if(xhr.status>=200&&xhr.status<300||xhr.status==304){
			alert(xhr.responseText);
		}else{
			alert("Request was unsuccessful:"+xhr.status);
		}
	}
};
xhr.open("get","example.php",true);
xhr.setRequestHeader("MyHeader","MyValue");
xhr.send(null);
```
------
#### 4.2.1.2 POST请求
+ **POST**请求是最常用的请求方法，用于向服务器发送应该保存的数据。每个POST请求都应该在请求体中携带要提交的数据。
+ 如下所示为一个设置了自定义头部（必选**Content-Type**，设为**“application/x-www-form-urlencoded"**）的POST请求。
```js
let xhr = new XMLHttpRequest();
function submitData(){
  xhr.onreadystatechange = function(){
    if(xhr.readyState===4){
      if(xhr.statu>=200&&xhr.status<300||xhr.status===304){
        alert(xhr.responseText);
      }else{
        alert("Request was unsuccessful:"+ xhr.status);
      }
    }
  }
};
xhr.open("post","postexample.php",true);
xhr.setRequestHeader("Content-Type","application/x-www-form-urlencoded");
let form = document.getElementById("user-info");
xhr.send(serialize(form));
// 来自ID为”user-info"的表单中的数据被序列化后发送给了服务器。
```
------
### 4.2.2 进度事件

+ 客户端和服务端之间的通信过程，有6个与进度相关的事件API。每次请求都会首先触发loadstart事件，之后是一个或多个progress事件，接着是error、abort或load中的一个，最后以loadend事件结束。
> 这些事件最初只针对XHR对象，现在也推广到了其他类似的API。

#### 4.2.2.1 load事件
+ **load**事件用于替代`readystatechange`事件。<u>load事件在响应接收完成后立即触发</u>。这样就不用检查`readystate`的属性了。
```js
let xhr = new XMLHttpRequest();
xhr.onload=function(){
	if((xhr.status>=200&&xhr.status<300)||xhr.status===304){
		alert(xhr.responseText);
	}else{
		alert("Request was unsuccessful:"+xhr.status);
	}
};
xhr.open("get","altevents.php",true);
xhr.send(null);
```

------
### 4.2.3 跨域资源共享

+ **同源策略**：默认情况下，XHR只能访问与发起请求的页面在同一个域内的资源。这个安全限制可以防止某些恶意行为。**协议、域名和端口号必须完全一致**，浏览器采用了这样的安全策略，即同源策略。违背同源策略的通信就是跨域。

+ **跨域资源共享CORS**：（Cross-Origin Resource Sharing）定义了浏览器与服务器如何实现跨源通信。CORS背后的基本思路是使用自定义的HTTP头部允许浏览器和服务器之间相互了解，以确定请求或响应应该成功还是失败。

  - 对于简单的请求，比如GET或POST请求，没有自定义头部，而且请求体是text/plain类型，这样的请求在发送时会有一个额外的头部叫Origin。Origin头部包含发送请求页面的源（协议、域名和端口），以便服务器确定是否为其提供响应。

  ```
  Origin:http://www.nczonline.net
  ```

  - 如果服务器决定响应请求，那么应该发送 `Access-Control-Allow-Origin`头部（后端代码实现），包含相同的源或者如果资源是公开的，那么就包含“*”。

  ```
  Access-Control-Allow-Origin:http://www.oczonline.net
  ```
> 对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。CORS需要浏览器和服务器同时支持。CORS需要浏览器和服务器同时支持。因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
>
> > 更多内容参考：[阮一峰：CORS详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)

+ **跨域XHR对象**：现代浏览器通过`XMLHttpRequest`对象原生支持CORS。在访问不同源的资源时，这个行为会被自动触发。要向不同域的源发送请求，可以使用标准XHR对象并给`open()`方法传入一个绝对URL。
- 出于安全考虑，跨域XHR对象也施加了一些额外限制:
    + 不能使用`setRequestHeader()`设置自定义头部。
    + 不能发动和接收cookie。
    + `getAllResponseHeaders()`方法始终返回空字符串。
```js
let xhr = new XMLHttpRequest():
xhr.onreadystatechange=function(){
	if(xhr.readyState===4){
		if((xhr.status>=200&&xhr.status<300)||xhr.status===304){
			alert(xhr.responseText);
		}else{
			alert("Request was unsuccessful:"+xhr.status);
		}
	}
};
// 给open方法传入绝对URL
xhr.open("get","http://www.somewhere-else.com/page/",true);
xhr.send(null);
```

------
### 4.2.4 替代性跨域技术

+ `CORS`出现之前，实现跨源Ajax通信是有些麻烦的。开发者需要依赖能够执行跨源请求的DOM特性，在不使用XHR对象的情况下发送某种类型的请求。虽然CORS已经得到广泛支持，但这些技术仍然没有过时，因为它们不需要修改服务器。

#### 4.2.4.1 图片探测

+ 图片探测是利用<img>标签实现跨域通信的最早的一种技术。任何页面都可以跨域加载图片而不必担心限制，这也是在线广告追踪的主要方式。可以动态创建图片，然后通过他们的`onload`和`onerror`事件处理程序得知何时收到响应。
+ 图片探测频繁用于跟踪用户在页面上点击操作或动态显示广告。当然，图片探测的缺点是只能发送`GET`请求和无法获取服务器响应的内容。这也是只能利用图片探测实现浏览器与服务器单向通信的原因。
```js
let img = new Image();
img.onload = img.onerror=function(){
	alert("Done");
};
// 设置完src属性后请求就开始了
img.src="http://www.example.com/test?name=Nicholas";
// 请求中发送了一个name参数
```

------
#### 4.2.4.2 JSONP

+ JSONP是“JSON with padding”的简写，是在web服务上流行的一种JSON变体。

+ JSONP格式包含两个部分：回调和数据。回调是在页面收到响应之后应该调用的函数，通常回调函数的名称是通过请求来动态指定的。而数据就是作为参数传给回调函数的JSON数据。下面是一个典型的JSONP请求

  ```
  http://freegeoip.net/json/?callback=handleResponse
  ```
+ **JSONP**是通过动态创建<script>元素并为src属性指定跨域URL实现的。此时的<script>元素与<img>元素类似，能够不受限制地从其他域加载资源。
```js
function handleResponse(response){
	console.log(`You're at IP address ${response.id},which is in ${response.city},${response.region_name}`);
};
let script = document.createElement("script");
//【跨域请求数据】，拿到请求回来的数据后使用handleResponse函数处理
script.src="http://freegeoip.net/json?callback=handleResponse";
document.body.insertBefore(script,document.body.firstChild);
```
+ 使用JSONP的优缺点？
  - JSON简单易用，相比于图片探测，使用JSONP可以直接访问响应，实现浏览器与服务器之间的双向通信。JSONP只适用于get请求。
  - JSONP是从不同的域拉取可执行代码，如果这个域中存在恶意代码，此时除了完全删除JSONP也没有其他办法。在使用不受控的web服务时，要确保是可以信任的。第二个缺点是不好确定JSONP请求是否失败，虽然HTML5规定了<script>元素的`onerror`事件处理程序，但还没有被任何浏览器实现。
> 详细资料可以参考：[《原生 jsonp 具体实现》](https://www.cnblogs.com/zzc5464/p/jsonp.html)、[《jsonp 的原理与实现》](https://segmentfault.com/a/1190000007665361#articleHeader1)
------
### 4.2.5 Fetch API

+ **Fetch API** 是一个获取资源的接口，它能执行XHR对象的所有任务，是下一代Ajax技术（或者说用来替代传统的Ajax技术）。XHR对象可以选择<a href="#xhrasync">异步或同步</a>，而`Fetch API`必须是异步。

+ Fetch API和XHR对象没有关系，而是原生js。

+ `fetch()`方法暴露在全局作用域中，包括主页面执行线程，模块和工作线程。调用这个方法，浏览器会向给定的URL发送请求。

#### 4.2.5.1 Fetch API的基本用法

+ （1）分派请求：

  - **fetch()** 只有一个必需的参数，URL。这个方法返回一个Promise实例对象。

  - 请求资源完成，资源可用时，promise对象会被处理成`Response对象`。通过这个对象可以获取相应的资源。
  ```js
  fetch('bar.txt')
  	.then((response)=>{
  		console.log(response)
  	});
  // Response {type:"basic",url:......}
  ```
  
+ (2) 读取响应：

  - 读取响应的最简单方式是获取纯文本内容，需要调用`text()`方法。`text()`方法返回一个promise对象，这个对象表示取得资源的完整内容。
  - `json()`方法返回一个promise对象，将获取到的数据转换成JSON

  ```js
  fetch('bar.txt')
  	.then((response)=>response.text())
  	.then((data)=>console.log(data));
  
  fetch('http://localhost:3000/json')
  	.then(response=>response.json())
  	.then(data=>{
    	var obj = JSON.parse(data);
    	console.log(obj.uname);
  })
  ```

+ (3) 处理状态码和请求失败

  - Fetch API支持通过Response 的**status**（状态码）和**statusText**（状态文本）属性来检查响应状态。成功获取响应的请求通常会产生值为200的状态；请求不存在的资源会产生值为404的状态码；请求的URL如果抛出服务器错误会产生值为500的状态码；跟随重定向时，响应对象的`redirected`属性会被设置为true，而状态码仍然是200。
  - 违反CORS、无网络连接、HTTPS配置错误及其他浏览器/网络策略问题都会导致响应对象被rejected
  - 可以通过url属性检查通过`fetch()`发送请求时的使用的**完整URL**。
  ```js
  fetch('//qux.com/bar')
  	.then((response)=>{
  		console.log(response.status);//200
  		console.log(response.statusText);//OK
  		console.log(response.ok);//true
  		console.log(response.url);//https://qux.com/bar
  	},err=>console.log(err));
  ```
+ (4) 只使用URL时，fetch()会发送GET请求，只包含最低限度的请求头。要进一步配置如何发送请求，需要传入第二个参数init对象。init对象里面可以配置请求头（headers）、请求方法（method）及cookies等。详见4.2.5.2。
------

#### 4.2.5.2 Fetch常见的请求模式（参数配置）

+ （1）**GET**请求
	- 通过url？的形式传参
	```js
	fetch("http://localhost:3000/books?id=123",{
		// get请求可以省略不写 默认是GET
		method:'get'
	})
	.then(data=>data.text())
	.then(data=>console.log(data));
	```
	- 通过/ 的形式传参(/id=456和后台配置有关)
	```js
	fetch("http://localhost/books/456")
	.then(data=>data.text())
	.then(data=>console.log(data));
	```
+ (2) **DELETE** 请求
	- 删除id=789的项
	```js
	fetch("http://localhost:3000/books/789",{
		method:'delete'
	})
	.then(data=>data.text())
	.then(console.log(data));
	```
+ (3) **POST** 请求
	- 请求体直接传string
	```js
	fetch('http://localhost:3000/books',{
		method:'post',
		body:'uname=lisi&pwd=123',
		headers:{
			'Content-Type':'application/x-www-form-urlencoded'
		}
	}).then(data=>data.text()).then(data=>console.log(data))
	```
	- 请求体传序列化的对象
	```js
	fetch('http://localhost:3000/books',{
		method:'post',
		body:JSON.stringify({
			uname:'zhangsan',
			pwd:'456'
		}),
		headers:{
			'Content-Type':'application/x-www-form-urlencoded'
		}
	}).then(data=>data.json()).then(data=>console.log(data))
	```
+ (4) **PUT**请求
	- 修改id是123的项
	```js
	fetch('http://localhost:3000/books/123',{
		method:'put',
		body:JSON.stringify({
			uname:'zhangsan',
			pwd:'asdvdf'
		}),
		headers:{
			'Content-Type':'application/json'
		}
	}).then(data=>data.json()).then(data=>console.log(data))
	```
------
### 4.2.6 axios库

+ `axios`是一个基于promise的一个HTTP库，可以用在浏览器和node.js中。
+ 可以使用`npm`包安装。
```bash
npm install axios
```
```js
import axios from 'axios';
// ...
```
> 更多内容参考：[axios中文文档](http://www.axios-js.com/zh-cn/docs/)
------
#### 4.2.6.1 axios库的特性
+ 从浏览器中创建XHR对象
+ 从node.js中创建http请求
+ 支持promise API
+ 支持拦截请求和响应
+ 能转换请求数据和相应数据
+ 自动转换JSON数据
+ 取消请求
+ 客户端支持犯防御XSRF
------
#### 4.2.6.2 axios库的常见使用模式？

+ (1) **GET**请求
	- get 不传参
	```js
	axios.get('http://localhost:3000/adata')
		.then(resp=>
	        // 注意data属性是固定用法，用于获取后台的实际数据
	        console.log(resp.data))
	```
	- get传参：以url?的形式传参
	```js
	axios.get('http://localhost:3000/axios?id=123')
		.then(resp=>console.log(resp.data))
	```
	- get传参：以restful形式url传参
	```js
	axios.get('http://localhost:3000/axios/123')
		.then(resp=>console.log(resp.data))
	```
	- get传参：以params对象形式传参
	```js
	axios.get('http://localhost:3000/axios',{
		params:{
			id:789
		}
	}).then(resp=>console.log(resp.data))
	```
+ (2) **DELETE**请求
	- **DELETE**请求的传参格式和GET请求一致
	```js
	axios.delete('http://localhost:3000/axios',{
		params:{
			id:111
		}
	}).then(resp=>console.log(resp.data))
	```
+ (3) **POST**请求
	- post传参：init对象
	```js
	axios.post('http://localhost:300/axios',{
		uname:'lisi',
		pwd:123
	}).then(resp=>console.log(resp.data))
	```
	- post传参：使用一个URLSearchParams类型的对象传参
	```js
	var params = new URLSearchParams();
	params.append('uname','zhangsan');
	params.append('pwd','1111');
	axios.post('http://localhost:3000/axios',params)
		.then(resp=>console.log(resp.data))
	```
+ (4) **PUT**请求
	- put请求的传参格式与post请求一样
	```js
	axios.put('http://localhost:3000/axios/123',{
		uname:'lisi',
		pwd:'3432'
	}).then(resp=>console.log(resp.data))
	```
------
#### 4.2.6.3 axios 全局配置

+ 配置baseURL。
```js
// 配置一个公用的基准URL
axios.defaults.baseURL="https://api.example.com"
```
+ 配置超时时间:如果请求超过了指定时间(ms)，请求将被中断
```js
axios.defaults.timeout=2500;
```
+ 配置token头
```js
axios.defaults.headers.common['Authorization']=AUTH_TOKEN;
```
+ 配置公共的post请求的自定义请求头
```js
axios.defaults.post['Content-Type']='application/x-www-form-urlencoded';
```

> 可以使用`axios.create(config)`方法新建一个axios实例。
> ```js
> var instance = axios.create({
>   baseURL: 'https://some-domain.com/api/',
>   timeout: 1000,
>   headers: {'X-Custom-Header': 'foobar'}
> });
> ```

----

#### 4.2.6.4 axios 拦截器
+ 请求拦截器
	- 请求发送前进行一些操作，例如在每个请求体里加上token，统一处理后以后再更改也非常容易。
	```js
	axios.interceptors.request.use(config=>{
		console.log(config.url);
		// 任何请求都会走到这里，发送请求前要做的事情
		config.headers.mytoken='nihao';
		// 【这里一定要return!!! 否则不成功】
		return config;
	},err=>console.log(err))
	```
+ 响应拦截器
	- 接收到响应后进行一些操作，例如在服务器返回登陆状态失效，需要重新登陆的时候，跳转到登录页。
	```js
	axios.interceptors.response.use(resp=>{
		// 接收到响应要做些什么
		var data = resp.data;
		return data;
	},err=>console.log(err));
	```
### 4.2.7 Web Socket

#### 4.2.7.1 什么是Web Socket？使用Web Socket有什么优缺点？

+ Web Socket 是一个在单个TCP连接上进行全双工、双向的**通信协议**。

+ Web Socket不能通过标准HTTP服务器实现，必须使用支持该协议的**专有服务器**。

+ Web Socket使用了自定义的协议，URL方案稍有变化：要使用`ws://`和`wss://`,分别表示不安全的连接和安全的连接。

+ 使用自定义协议而非HTTP协议的好处是，客户端与服务器端之间可以发送非常少的数据，不会对HTTP造成任何负担。使用更小的数据包让Web Socket非常适合带宽和延迟问题比较明显的移动应用。使用自定义协议的缺点是，定义协议的时间，比定义JS API要长。Web Socket得到了所有主流浏览器的支持。

+ Web Socket **不受同源策略的限制**。可以打开到任意站点的连接，至于是否与来自特定源的页面通信，则完全取决于服务器。

#### 4.2.7.2 如何使用Web Socket建立网络连接？

+ 浏览器会在初始化Web Socket对象后立即建立连接。Web Socket也有一个`readyState`属性表示当前状态。这个值和XHR中的不一样。
	- WebSocket.OPENING(0):连接正在建立。
	- WebSocket.OPEN(1):连接已建立。
	- WebSocket.CLOSING(2):连接正在关闭。
	- WebSocket.CLOSE(3):连接已关闭。
	
+ 任何时候都可以使用`close()`方法关闭Web Socket连接。

+ 要向服务器发送数据，使用`send()`方法并传入一个字符串、ArrayBuffer或Blob.

+ Web Socket对象在连接生命周期中有可能触发3个其他事件。
	- open:在连接成功时触发。
	- error:发生错误时触发，连接无法存续。
	- close:连接关闭时触发。
```js
let socket = new WebSocket('ws://www.exmple.com/server.php');
socket.send("Hello World");
socket.onclose=function(){
  alert("Connection closed");
}
socket.close();
```

-------

## 4.3 网络请求面试题

### 4.3.1 你了解哪些跨域解决方案？

+ 1. 图片探测：任何页面使用<img>元素都可以跨域加载图片而不必担心限制，这也是在线广告追踪的主要方式。
  
+ 2. JSONP：它的主要原理是通过动态构建 script  标签来实现跨域请求，因为浏览器对 script 标签的引入没有跨域的访问限制 。通过在请求的 url 后指定一个回调函数，然后服务器在返回数据的时候，构建一个 json 数据的包装，这个包装就是回调函数，然后返回给前端，前端接收到数据后，因为请求的是脚本文件，所以会直接执行，这样我们先前定义好的回调函数就可以被调用，从而实现了跨域请求的处理。这种方式只能用于 get 请求。支持老式浏览器，可以向不支持CORS的网站请求数据。
  
+ 3. CORS: CORS 是一个 W3C 标准，全称是"跨域资源共享"。CORS 需要浏览器和服务器同时支持，它允许浏览器向跨源服务器，发出[`XMLHttpRequest`](http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html)请求，从而克服了AJAX只能[同源](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)使用的限制。现代浏览器都支持该功能，因此我们只需要在服务器端配置就行。浏览器将 CORS 请求分成两类：简单请求和非简单请求。
	- 对于简单请求，浏览器直接发出 CORS 请求。具体来说，就是会在头信息之中，增加一个 `Origin` 字段。`Origin` 字段用来说明本次请求来自哪个源(协议、域名和端口)。服务器根据这个值，决定是否同意这次请求。对于如果 `Origin` 指定的源，不在许可范围内，服务器会返回一个正常的 HTTP 回应。浏览器发现，这个回应的头信息没有包含 `Access-Control-Allow-Origin` 字段，就知道出错了，从而抛出一个错误，ajax 不会收到响应信息。如果成功的话会包含一些以 Access-Control- 开头的字段。
	
	- 非简单请求，浏览器会先发出一次`预检请求`，来判断该域名是否在服务器的白名单中，如果收到肯定回复后才会发起请求。
	> 现代浏览器通过XHR对象原生支持CORS。在尝试访问不同源的请求时，这个行为会被自动触发。要向不同域的源发送请求时，可以使用标准的XHR对象并给open()方法传入一个绝对的URL。

+ 4. 使用`Web Socket`协议：Web Socket协议是一个自定义的协议，不受同源策略的限制。
+ 5. 让服务器来代理跨域的访问请求，有跨域请求时发送请求给后端服务器，让后端代为请求，然后最后将获取的结果发返回。服务器向服务器发送请求不受同源策略的限制。
------
### 4.3.2 什么是Token？
+ 会话状态：使用者处于网页使用的状态。
+ 传统开发过程，服务器如何验证用户信息？

  - 服务器端会话，登陆之后，服务器先判断用户名和密码是否存在，如果不存在就提示“用户名不存在”；

  - 如果用户登录成功，创建一个session，记录登录时间，session设置了有效时长。如果用户登录了一直没有做任何操作，到时间就过期了，提示重新登陆。用户只要请求，就会判断session里面的登陆状态是否过期。
+ 前后端分离式开发（不使用token）
  - 前后端分离，没有session状态。
  - 登陆成功之后，验证了是否有用户名，还是会创建session。但是这个session只有服务端有，后端会将session对应的key的id，通过接口返回给前端，前端存起来（本地存储或者cookie）！服务器端会在数据库中留下记录。此时，服务器内存环境中不再保留session。
  - 每次发送请求就携带session id 发送过去，服务器端验证。验证时，服务器会在数据库中查询比对。
+ Token的机制：
  - 当用户登陆服务器验证成功后，服务器根据当前的时间戳、用户id和秘钥字符串（开发者定义）三者通过加密算法生成一个字符串，这个字符串叫做令牌，又称之为token。服务器会把用户信息和token一起返回给前端（客户端）。
  - 服务器返回token给客户端，客户端将用户信息和token保存在本地存储，实现持久化。跳转到对应页面。
  - 当前端再次发送请求（请求头已添加token）时，所有的请求接口（除了登陆或者一些白名单地址）都要进行token验证。
  - token验证：从请求头解析出token，对token进行解密。能判断出时间是否过期，是否不符合算法，提示token不正确，解析出用户id，去数据库查找id是否存在，从而判断用户是否存在。如果验证不通过，返回前端token非法（没有，错误，过期）；如果通过验证，就执行业务逻辑，将数据插入到数据库中，且提示前端添加成功。
  - 这如果服务器返回token验证失败，前端就需要重新登陆。
+ 使用Token的目的：Token的目的是为了减轻服务器的压力，减少频繁的查询数据库，使服务器更加健壮。


------
# 5. 客户端存储

## 5.1 Cookie

+ HTTP cookie通常叫做cookie，是一个HTTP的规范。是服务器提供的一种用于维护会话信息状态的数据，通过服务器发送到浏览器，浏览器保存在本地，当下一次有同源的请求时，将保存的cookie添加到请求头，发送给服务器。可以用来实现记录用户登录状态等的功能。cookie一般存储不超过4KB（4096个字节）的数据，并且只能被同源的网页所共享访问。

+ Cookie的构成：

	- 服务器端可以使用 `Set-Cookie` 的响应头部来配置 cookie 信息。一条cookie 包括了7个参数，如：名称`name`、值 `value`、域`domain`、路径`path`、过期时间`expires` 、安全标志`secure`和`HTTP-only`。domain 和 path 一起限制了 cookie 能够被哪些 url 访问。secure 规定了 cookie 只能在确保安全的情况下传输，HttpOnly 规定了这个 cookie 只能被服务器访问，不能使用 js 脚本访问。

    ```http
    HTTP/1.1 200 OK
    Content-Type:text/html
    Set-Cookie:name=value;expires=Mon,22-Jan-07 07:10:24 GMT;domain=.wrox.com;path=/ ; secure
    Other-header:other-header-value
    ```

	- 浏览器存储会话信息，在之后的每个请求中通过HTTP头部`cookie`再将它们发送回服务器。注意，域，路径，过期时间和secure标志用于高速浏览器在什么情况下应该在请求中包含cookie。这些参数并不会随请求发送给服务器，实际发送的只有cookie的名/值对。
	  ```http
	  GET /index.js1 HTTP/1.1
	  Cookie:name=value
	  Other-header:other-header-value
	  ```
> 详细资料可以参考：[《HTTP cookies》 ](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)、[《聊一聊 cookie》 ](https://segmentfault.com/a/1190000004556040)   

------
## 5.2 Web Storage

+ Web Storage规范的两个目标：
  - 提供在**cookie之外**的存储会话数据的途径；
  - 提供**跨会话持久化存储大量数据**的机制。
+ Web Storage第二版定义了两个对象:`localStorage`和`sessionStorage`。`localStorage`是永久存储机制，`sessionStorage`是跨回话的存储机制。这两种API提供了在浏览器中不受页面刷新影响而存储数据的方式。
+ Storage 类型
	- Storage类型用于保存名/值对，直至存储空间上限。Storage实例增加了一些方法和属性：
		+ clear():删除所有值
		+ getItem(name):取得给定name的值
		+ key(index):取得给定数值位置的名称
		+ removeItem(name):删除给定name的名/值对
		+ setItem(name,value):设置给定的那么的值
		+ length:确定Storage对象中保存了多少名/值对
	- Storage类型只能存储字符串
------
### 5.2.1 sessionStorage
+ `sessionStorage`对象是`Storage`类型的实例。
+ `sessionStorage` 对象**只存储会话数据**，这意味着数据只会存储到浏览器关闭。跟浏览器关闭会消失的会话`cookie`类似。存储在`sessionStorage`中的数据不受页面刷新影响，可以在浏览器崩溃并重启后恢复。
+ 给`sessionStorage`对象添加数据
```js
// 使用方法
sessionStorage.setItem("name","Nicholas");
// 使用属性赋值
sessionStorage.book=”Professional Javascript";
```
+ 读取`sessionStorage`对象上的数据
```js
// 使用方法
let name = sessionStorage.getItem("name");
//使用属性获取
let book = sessionStorage.getItem("book");
```
+ 删除`sessionStorage`对象上的数据
```js
// 使用delete删除
delete sessionStorage.name;
// 使用方法删除值
sessionStorage.removeItem("book");
```
------
### 5.2.2 localStorage
+ `localStorage`对象是`Storage`类型的实例。

+ `localStorage`是客户端**持久存储数据**的机制。要访问同一个`localStorage`对象，页面必须来之用一个域，相同的端口和相同的协议。

+ `localStorage`对象的使用方法同`sessionStorage`。

+ 存储在`localStorage`对象上的数据会保留到通过Javascript删除或者用户清除浏览器缓存。`localStorage`对象上的数据不受页面刷新的影响，也不会因为关闭窗口、标签页或重启浏览器而丢失。 

> **localStorage**对象有什么特点？ 
>
>  1. localStorage的值为string类型，这就说明我们在使用的时候可能需要进行类型转换。
>  2. 在浏览器隐私模式下不可读取。
>  3. 不同浏览器无法共享localStorage，相同浏览器的不同页面间可以共享相同的 localStorage（页面属于相同域名和端口），但是不同页面或标签页间无法共享sessionStorage的信息。这里需要注意的是，页面及标 签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，那么他们之间是可以共享sessionStorage的。
>  4. localStorage不能被爬虫获取
>  5. 存储过多数据会导致页面变卡，因为localStorage的本质是对字符串的读取
------
### 5.2.3 存储事件
+ 每当Storage对象发生变化时，都会在文档上触发`storage`事件。对于`sessionStorage`和`localStorage`上的任何更改都会触发`storage`事件。但是storage事件不会区分两者。
------
### 5.2.4 限制
+ Web Storage的限制取决于特定的浏览器。一般来说，客户端数据的大小限制是按照每个源（协议、域名和端口）来设置的，因此每个源有固定大小的数据存储空间。
+ 不同浏览器给`localStorage`和`sessionStorage`设置了不同的空间限制，大多数限制为每个源5MB。
------
#### 5.2.5 [面试题] 试比较cookies，localStorage，sessionStorage的异同？
+ localStorage:仅在客户端存储，不参与服务器通信，用于浏览器缓存数据。存储大小一般为5M，如果不是人为清除，那么即使是关闭浏览器也会一直存在; 同源标签页（窗口）之间共享。

+ sessionStorage:仅在客户端存储，不参与服务器通信，用于浏览器缓存数据。存储大小一般为5M，会话级存储，也就是说如果关闭当前页面或者浏览器那么就会清除; 同源标签页（窗口）之间**不共享**。

+  cookie:由服务器端生成，客户端存储，参与服务器通信，存储大小限制为4KB,可设置生命周期，在设置的生命周期内有效。如果客户端保存了cookie，http请求一定会带上（而两个storage可以由脚本选择性地提交）。 同源标签页（窗口）之间共享。

  
------
# 6. 模块化
## 6.1 模块化开发怎么理解？
+ 一个模块是实现一个特定功能的一组方法。在最开始的时候，js 只实现一些简单的功能，所以并没有模块的概念，但随着程序越来越复杂，代码的模块化开发变得越来越重要。由于函数具有独立作用域的特点，最原始的写法是使用函数来作为模块，几个函数作为一个模块，但是这种方式容易造成全局变量的污染，并且模块间没有联系。后面提出了对象写法，通过将函数作为一个对象的方法来实现，这样解决了直接使用函数作为模块的一些缺点，但是这种办法会暴露所有的所有的模块成员，外部代码可以修改内部属性的值。现在最常用的是立即执行函数的写法，通过利用闭包来实现模块私有作用域的建立，同时不会对全局作用域造成污染。
> 详细资料可以参考：[《浅谈模块化开发》](https://juejin.im/post/5ab378c46fb9a028ce7b824f) 、[《Javascript 模块化编程（一）：模块的写法》](http://www.ruanyifeng.com/blog/2012/10/javascript_module.html)、[《前端模块化：CommonJS，AMD，CMD，ES6》](https://juejin.im/post/5aaa37c8f265da23945f365c)、[《Module 的语法》](http://es6.ruanyifeng.com/#docs/module)、[《JS 模块加载器加载原理是怎么样的？》](https://www.zhihu.com/question/21157540)

## 6.2 四种模块化规范
### 6.2.1 CommonJS
+ CommonJS规范主要用于在服务器端实现模块化代码组织。以同步的方式来引入模块的，因为在服务端文件都存储在本地磁盘，所以读取非常快，所以以同步的方式加载没有问题。
+ CommonJS模块定义需要使用`require()`指定依赖，使用`module.exports`对象定义自己的公共API。
```js
// 定义一个模块A
var moduleB = require('./moduleB');
module.exports = {
	stuff:moduleB.doStuff();
};
```
+ 所有CommonJS风格的实现共同之处是模块不会指定自己的标识符，它们的标识符由其模块文件层级中的位置决定。指向模块定义的路径可能引用一个目录，也可能是一个JS文件。
+ 在Node环境中，使用`import`命令加载CommonJS模块
```js
import mA from './modulA';
// mA = {
// 	 stuff:moduleB.doStuff();
// }
```
------
### 6.2.2 AMD
+ AMD(异步模块定义-Asynchronous Module Definition)模块定义系统以浏览器为目标执行环境，需要考虑网络延迟的问题。
+ AMD的一般策略是让模块声明自己的依赖，而运行在浏览器中的模块系统会按需获取依赖，并在依赖加载完成后立即执行依赖它们的模块。
+ AMD模块实现的核心是用函数包装模块定义。这样可以防止声明全局变量，并允许加载器库控制何时加载模块。
+ AMD方案，采用异步加载的方式来加载模块，模块的加载不影响后面语句的执行，所有依赖这个模块的语句都定义在一个回调函数里，等到加载完成后再执行回调函数。`require.js`实现了 AMD 规范。
+ 与CMD不同，AMD支持可选地为模块指定字符串标识符。
```js
// ID为“moduleA”的模块定义。moduleA依赖moduleB
define('moduleA',['moduleB'],function(moduleB){
	return{
		stuff:moduleB.doStuff();
	}
})
```
+ AMD也支持`require`和`exports`对象，通过它们可以在AMD模块工厂函数内部定义CommonJS风格的模块。可以像请求模块一样请求它们。
```js
define('moduleA',['require','exports'],function(require,exports){
	var moduleB = require('moduleB');
	exports.stuff=moduleB.doStuff();
})
```
------
### 6.2.3 CMD
+ CMD(通用模块定义-Common Module Definition)同AMD一样，也是为了解决异步模块加载的问题而出现的规范。
+ CMD规范中，一个模块就是一个文件。
```js
define(function(require,exports,module){
	// 模块代码
	// require是可以把其他模块导入进来的一个参数;而exports是可以把模块内的一些属性和方法导出的;module 是一个对象，上面存储了与当前模块相关联的一些属性和方法。
})
```
+ CMD推崇就近依赖，只有在用到某个模块的时候再去require。
------
#### 6.2.3.1 AMD与CMD规范的区别？
+ 1. 在模块定义时对依赖的处理不同。AMD 推崇**依赖前置**，在定义模块的时候就要声明其依赖的模块。而 CMD 推崇**就近依赖**，只有在用到某个模块的时候再去 require。

+ 2. 第二个方面是对依赖模块的执行时机处理不同。AMD 在依赖模块加载完成后就直接执行依赖模块，依赖模块的执行顺序和我们书写的顺序不一定一致。而 CMD在依赖模块加载完成后并不执行，只是下载而已，等到所有的依赖模块都加载好后，进入回调函数逻辑，遇到 require 语句的时候才执行对应的模块，这样模块的执行顺序就和我们书写的顺序保持一致了。
```js
// AMD 默认推荐
define(["./a", "./b"], function(a, b) {
  // 依赖必须一开始就写好
  a.doSomething();
  // 此处略去 100 行
  b.doSomething();
  // ...
});

// CMD
define(function(require, exports, module) {
  var a = require("./a");
  a.doSomething();
  // 此处略去 100 行
  var b = require("./b"); // 依赖可以就近书写
  b.doSomething();
  // ...
});
```
> 详细资料可以参考：[《前端模块化，AMD 与 CMD 的区别》](https://juejin.im/post/5a422b036fb9a045211ef789)
------
### 6.2.4 ES6 模块化
+ ES6最大的一个改进就是引入了模块化规范，是集AMD和CommonJS之大成者。
+ ES6模块化的设计思想是尽量静态化，使得编译时就能确定模块的依赖关系，以及输入输出的变量。CommonJS和AMD模块都只能在运行时确定这些东西。
+ ES6的模块自动采用严格模式。
+ 模块标签
	- 带有`type="module"`属性的\<script>标签会告诉浏览器相关代码应该作为模块执行，而不是作为传统的脚本执行。
	- 解析到\<script type="module">标签后会立即下载模块文件，但执行会延迟到文档解析完成。\<script type="module">标签在页面中出现的顺序就是它们的执行顺序。
	- 给模块添加`async`属性：不仅模块的执行顺序不再与\<script>标签在页面中的顺序绑定，模块也不会等待文档解析完成才执行。
+ 模块导出
	- `export`关键字用于声明一个值(可以是变量、函数或类）为命名导出。导出语句必须在模块顶级，不能嵌套在某个块中。
	```js
	export const foo = 'foo';
	```
	- 命名导出，可以在同一行内执行变量声明。
	- 可以用`export`命令加大括号指定一组需要导出的值。使用`as`关键字重命名对外接口。
	```js
	export {foo,bar};
	export {v1 as streamV1};
	```
	- 默认导出，使用`default`关键字讲一个值声明为默认导出，摸个模块只能有一个默认导出。指定默认导出后，这个模块本身就是被导出的值。
	```js
	const foo='foo';
	export default foo;
	// 等同于
	// export {foo as default};
	```
+ 模块导入
	- 模块可以通过使用`import`关键字使用其他模块导出的值。与`export`类似，import必须出现在模块的顶级。
	
	- `import`语句具有提升效果，但还是推荐把导入语句放到模块顶部位置。
	
	- 多次执行同一句`import`命令只会执行一次。
	
	- `from`后的模块标识符可以是相对于当前模块的相对路径，也可以是指向模块文件的绝对路径。它必须是字符串，不能是动态计算的结果。
	- 可以使用
	- 如果在浏览器中通过标识符原生加载模块，则文件名必须带有`.js`扩展名。
	```js
	import foo from './foo.js';
	import bar, {baz, bat as bag} from './foo.js';
	import foo, * as Foo from './foo.js'
	```
------
#### 6.2.4.1 CommonJS和ES6的模块化差异？
- 1.CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。CommonJS 模块输出的是值的拷贝，也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。ES6 模块的运行机制与 CommonJS 不一样。JS 引擎对脚本静态分析的时候，遇到模块加载命令 import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。

- 2.CommonJS 模块是运行时加载，ES6 模块是编译时输出接口。CommonJS 模块就是对象，即在输入时是先加载整个模块，生成一个对象，然后再从这个对象上面读取方法，这种加载称为“运行时加载”。而 ES6 模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成。
------

# 7. 工程化

## 7.1 Git

### 7.1.1下载安装

+ 从git官网下载安装包，安装完毕后就可以使用命令行的 git 工具，在开始菜单里找到"Git"->"Git Bash"，会弹出 Git 命令窗口，你可以在该窗口进行 Git 相关命令行的操作。

> 具体可参考https://www.runoob.com/git/git-install-setup.html
------
### 7.1.2 全局配置环境

+ 配置个人用户名和电子邮箱

```bash
git config –globle user.name “runoob”
git config –globle user.email text@runoob.com
```

+ 配置完毕后，可以通过$ git config –list命令查看所有的配置信息。也可直接查询某个环境变量的信息。

```bash
git config user.name
git config user.email
```
------
### 7.1.3 查看工作区状态

```bash
git status
```

- 状态一：修改了没有添加到缓存区（红色），此时可以通过git diff 查看修改了的内容，“-”号是修改前，“+”号是修改后，第一个加号后修改的前一行。第二个加号是修改的内容。
- 状态二：修改了添加到缓存区（绿色）
- 状态三：On branch master nothing to commit, work tree clean 表明无修改内容
------
### 7.1.4 添加文件到git仓库

+ 分两步：
	- 把修改的修改添加到版本库里的暂存区，可以单独添加某个文件，可多次使用
	- 把暂存区的所有内容提交到当前分支，提交的说明一定要写（字符串加双引号）

```bash
git add <file>
git commit -m <message>
```
------
### 7.1.5 本地同步更新远程分支

```bash
git pull
```
+ 如果项目是多人合作的，那么就需要在拉去别人更新的代码合并到本地。Git会自动合并本地代码。
------
### 7.1.6 把仓库中的代码推送到远程分支

```bash
git push
```
------
### 7.1.7 撤销修改

- 场景一：修改了文件但是未被add

```bash
git checkout -- <file>
```

- 场景二：修改了工作区内容，还添加到了暂存区时，想丢弃修改，分两步

```bash
git reset HEAD <file> 就回到了场景一
git checkout -- <file>
```

- 场景三：修改文件已被commit,但是没有推送到远程库，想要撤销本次提交，只能切换版本

```bash
git reset --hard HEAD^
```

### 7.1.8 从远程分支拉取项目

```bash
git clone SSH/HTTPS地址 -b <分支名>
```
------
### 7.1.9 分支管理

+ 当前分支作业时

```
1)查看分支：git branch
2)创建分支：git branch <name>
3)切换分支：git checkout <name>
4)创建+切换分支：git checkout -b <name>
5)合并某分支到当前分支：git merge <name>
6)删除分支git branch -d <name>
```

+ 临时切换分支作业时

```
1)暂存分支工作状态： git stash [注意：stash命令必须是在已经add之后或已经commit之后]
2)查看分支存储的工作状态： git stash list
3)恢复分支工作状态： git stash apply
4)删除分支存储的工作状态：git stash drop
5)恢复并删除分支存储工作状态：git stash pop
```

+ 切换远程分支：当前分支branch1工作，现在需要在分支branch2上工作，则需要切换

```bash
git fetch origin branch2(分支名)
git checkout branch2
```
-------
###  7.1.10 常见面试题
#### 7.1.10.1 git 与 svn 的区别在哪里？
+  git 和 svn 最大的区别在于 git 是分布式的，而 svn 是集中式的。因此我们不能再离线的情况下使用 svn。如果服务器出现问题，我们就没有办法使用 svn 来提交我们的代码。
svn 中的分支是整个版本库的复制的一份完整目录，而 git 的分支是指针指向某次提交，因此 git 的分支创建更加开销更小并且分支上的变化不会影响到其他人。svn 的分支变化会影响到所有的人。svn 的指令相对于 git 来说要简单一些，比 git 更容易上手。
> 详细资料可以参考: 
>  [《常见工作流比较》](https://github.com/geeeeeeeeek/git-recipes/wiki/3.5-%E5%B8%B8%E8%A7%81%E5%B7%A5%E4%BD%9C%E6%B5%81%E6%AF%94%E8%BE%83)、[《对比 Git 与 SVN，这篇讲的很易懂》](https://juejin.im/post/5bd95bf4f265da392c5307eb)、[《GIT 与 SVN 世纪大战》](https://blog.csdn.net/github_33304260/article/details/80171456)、[《Git 学习小记之分支原理》](https://www.jianshu.com/p/e8ad60710017)
------
#### 7.1.10.2 git pull 和 git fetch 的区别？
+   git fetch 只是将远程仓库的变化下载下来，并没有和本地分支合并。git pull 会将远程仓库的变化下载下来，并和当前分支合并。
>  [《详解 git pull 和 git fetch 的区别》](https://blog.csdn.net/weixin_41975655/article/details/82887273)
------
#### 7.1.10.2 git rebase 和 git merge 的区别
+  git merge 和 git rebase 都是用于分支合并，关键在 commit 记录的处理上不同。git merge 会新建一个新的 commit 对象，然后两个分支以前的 commit 记录都指向这个新 commit 记录。这种方法会保留之前每个分支的 commit 历史。git rebase 会先找到两个分支的第一个共同的 commit 祖先记录，然后将提取当前分支这之后的所有 commit 记录，然后将这个 commit 记录添加到目标分支的最新提交后面。经过这个合并后，两个分支合并后的 commit 记录就变为了线性的记录了。
> [《git rebase 和 git merge 的区别》](https://www.jianshu.com/p/f23f72251abc)、[《git merge 与 git rebase 的区别》](https://blog.csdn.net/liuxiaoheng1992/article/details/79108233)
------
## 7.2 Webpack
> 主要基于Webpack4
> 尚硅谷PDF

### 7.2.1 Webpack 简介

#### 7.2.1.1 Webpack是什么？

+ Webpack 是一种前端资源构建工具，一个静态模块打包器。在Webpack看来，前端的所有资源文件（js、json、css、less、img、html......)都会作为模块处理。它将根据模块的依赖关系进行静态分析，打包生成对应的静态资源（bundle）。
------
#### 7.2.1.2 Webpack的五个核心概念

+  (1) **Entry** : 指示以哪个文件为入口起点开始打包，分析构建内部依赖图。
+  (2) **Output** : 指示webpack打包后的资源bundles输出到什么位置，以及如何命名。
+  (3) **Loader** : Loader 让webpack能够去处理那些非Javascript文件（webpack自身职能理解Javscript）。
+  (4) **Plugins** : Plugins(插件)可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量等。
+  (5) **Mode** : Mode(模式) 指示webpack使用响应的模式来打包。
	- development : 会将DefinePlugin中 process.env.NODE_ENV 的值设置为development。适合本地调试代码。
	- production :  会将DefinePlugin中的process.env.NODE_ENV 的值设置为production。适合优化上线代码。
-------
#### 7.2.2 Webpack的基本使用

+  1. 初始化 package.json
```bash
npm init
```
+ 2. 下载并安装webpack
```bash
# 推荐本地安装，-D 写入devDependencies
npm install webpack webpack-cli -D 
```
+ 3. 功能
	- webpack能编译打包js和json文件
	- webpack能将es6的模块化语法转换成浏览器能识别的语法
	- webpack能压缩代码
------
### 7.2.3 开发环境配置
> 自定义webpack配置要依托于自己在项目根目录创建的文件`webpack.config.js`。
+ webpack构建工具基于nodejs平台运行，模块化默认使用`commonJS`。
#### 7.2.3.1 基础配置
+ 配置内容`webpack.config.js`
```js
const {resolve} = require('path') // node 内置核心模块，是一个用来拼接绝对路径的方法。
module.exports={
  entry:'./src/js/index.js', // 入口文件
  output:{
    filename:'./built.js', // 输出文件名
    path: resolve(__dirname,'build/js') // __dirname 表示当前文件的绝对路径
  },
  mode:'development' //开发环境
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.3.2 打包CSS样式

+ 下载安装`Loader`包
```bash
# 这里处理.css和.less 文件
npm i css-loader style-loader less-loader less -D
```
+ Loader 功能介绍
	- `css-loader`: 将css文件变成commonJS模块加载到js中，里面是样式字符串，通过eval函数来识别引用。
	- `style-loader`: 创建\<style>标签，将js中的样式资源插入到标签中，并添加到\<heade>标签下生效。
	- `less-loader`: 将less编译成css文件。注意要依托于`less`。
+ 配置内容`webpack.config.js`
```js
const {resolve} = require('path');
module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  // 配置loader
  module:{
    rules:[
      // 不同类型的文件要配置不同的loader来处理
      {
        test:/\.css$/,
        use:[
          // 多个loader从下往上执行
          'style-loader',
          'css-loader'
        ]
      },
      {
        test:/\.less$/,
        use:[
          'style-loader',
          'css-loader',
          'less-loader'
        ]
      }
    ]
  },
  plugins:[],
  mode:'development'
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.3.3 打包HTML资源
+ 下载安装`plugin`
```bash
# npm5之后，--save 可写可不写，安装包时默认会修改package.json
npm install --save -D html-webpack-plugin
```
+ `plugin`功能介绍：
	- html-webpack-plugin：默认会创建一个空的html，自动引入打包输出的所有资源（js、css），可以配置一个模板html文件， 实现依托模板打包资源的功能。
+ 配置内容`webpack.config.js`
```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      // 配置loader
    ]
  },
  // 配置plugins
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    })
  ],
  mode:'development'
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.3.4 打包图片资源
+ 下载安装`loader`
```bash
npm install html-loader url-loader file-loader -D
```
+ loader功能介绍
	- 针对css中`background-img`引入的图片，使用`url-loader`处理,但`url-loader`依赖于`file-loader`。
	- 针对html中直接使用<img> 标签引入的图片，使用`html-loader`处理。
+ 配置内容`webpack.config.js`。
```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      {
        test:/\.less$/,
        use:[
          'style-loader','css-loader','less-loader'
        ]
      },
//----------------------------------------------------------      
      // 处理CSS引入的图片
      {
        test:/\.(jpg|png|gif)$/,
        loader:'url-loader',
        options:{
          limit:8*1024, //8kb以下的图片会被处理成base64
          name:'[hash:10].[ext]',// 给文件重命名，取图片的hash前10位，并取文件原始扩展名
        }
      },
      // 处理html里img标签引入的图片
      {
        test:/\.html$/,
        loader:'html-loader'
      }
//--------------------------------------------------------------      
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    })
  ],
  mode:'development'
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.3.5 打包其他资源
+ 打包除js、css、html、less之外的资源。比如，针对静态字体文件。
+ loader:`file-loader`。
+ 配置内容`webpack.config.js`。
```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      {
        test:/\.css$/,
        use:[
          'style-loader','css-loader'
        ]
      },
//----------------------------------------------------------      
      // 打包其他资源
      {
        exclude:/\.（css|js|html|less)$/,
        loader:'file-loader',
        options:{
          name:'[hash:10].[ext]'
        }
      },
//--------------------------------------------------------------      
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    })
  ],
  mode:'development'
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.3.6 开发服务器devServer
+ devServer 用来完成自动化，可以自动化编译，自动打开浏览器。
+ devServer 只会在内存中编译打包，用于开发环境测试代码。不会有任何输出。
+ 依赖包：`webpack-dev-server`
+ 配置内容`webpack.config.js`：
```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      {
        test:/\.css$/,
        use:[
          'style-loader','css-loader'
        ]
      },
      {
        exclude:/\.（css|js|html|less)$/,
        loader:'file-loader',
        options:{
          name:'[hash:10].[ext]'
        }
      },    
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    })
  ],
  mode:'development',
  devServer:{
    contentBase:resolve(__dirname,'build'), //构建后路径
    compress:true,// 启动gzip压缩代码
    port:3000, //启动端口号
    open:true, // 自动打开浏览器
  }
}
```
+ 运行指令
```bash
# 本地启动，启动后会一致运行
npx webpack-dev-server
```
------
#### 7.2.3.7 完整的开发环境配置✅
+ 实现打包样式、HTML、图片及其他资源，并配置devServer以自动化测试代码。

```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      {
        test:/\.css$/,
        use:['style-loader','css-loader']
      },
      {
        test:/\.less$/,
        use:['style-loader','css-loader','less-loader']
      },
      {
        test:/\.(jpg|png|gif)$/,
        loader:'url-loader',
        options:{
          limit:8*1024, 
          name:'[hash:10].[ext]',
          outputPath:'imgs'
        }
      },
      {
        test:/\.html$/,
        loader:'html-loader'
      }
      {
        exclude:/\.(css|js|html|less|jpg|png|gif)$/,
        loader:'file-loader',
        options:{
          name:'[hash:10].[ext]',
          outputPath:'media'
        }
      }, 
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    })
  ],
  mode:'development',
  devServer:{
    contentBase:resolve(__dirname,'build'), //构建后路径
    compress:true,// 启动gzip压缩代码
    port:3000, //启动端口号
    open:true, // 自动打开浏览器
  }
}
```
+ 运行指令
```bash
# 本地启动，启动后会一致运行
npx webpack-dev-server
```
------
### 7.2.4 生产环境配置

#### 7.2.4.1 提取CSS为独立文件

+ 生产环境css要打包成单独的文件，而不再把css通过<style>标签嵌入html中。所以不再使用`style-loader`。

+ 使用`MiniCssExtractPlugin.loader`可以将js中的css剥离出来形成单独的css文件。

+ 下载安装插件
```bash
npm install -S -D mini-css-extract-plugin
```
+ 配置内容`webpack.config.js`
```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      {
        test:/\.css$/,
        use:[
          // 取代style-loader，将css从js中剥离出来，形成独立文件
          MiniCssExtractPlugin.loader,
          // 将css文件整合到js中
          'css-loader'
        ]
      },
      
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    }),
    new MiniCssExtractPlugin({
    // 对输出的css重命名
      filename:'css/built.css'
    })
  ],
  mode:'production'
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.4.2 CSS兼容性处理

+ `postcss`是一个用 JavaScript 工具和插件转换 CSS 代码的工具。
+ 使用`postcss`对css兼容性处理，要依赖两个包，`postcss-loader`和`postcss-preset-env`。
+ 下载安装包。
```bash
npm install -S -D postcss-loader postcss-reset-env
```
+ 配置内容`webpack.config.js`
```js
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
// 设置nodejs的环境变量
// process.env.NODE_ENV = 'development';
module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      {
        test:/\.css$/,
        use:[
          MiniCssExtractPlugin.loader,
          'css-loader',
          {
            loader:'postcss-loader',
            options:{
              ident:'postcss',
              plugins:()=>{
                // postcss的插件
                // postcss-preset-env：帮助postcss找到package.json中的browerslist里面的配置，通过配置加载指定的css兼容性样式
                require('postcss-preset-env')()
              }
            }
          }
        ]
      },
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    }),
    new MiniCssExtractPlugin({
    // 对输出的css重命名
      filename:'css/built.css'
    })
  ],
  mode:'development'
}
```
+ 修改`package.json`。
```json
//......
"browerslist":{
  // 如果使用开发模式，需要改变node环境变量。
	"development":[
		"last 1 chrom version",
		"last 1 firefox version",
		"last 1 safari version"
	],
  // 默认针对生产环境
  "production":[
    ">0.2%",
    "not dead",
    "not op_mini all"
  ]
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.4.3 压缩CSS

+ 下载安装依赖插件Plugin
```bash
npm install -S -D optimize-css-assets-webpack-plugin
```
+ 配置内容`webpack.config.js`
```js
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetWebpackPlugin = require('optimize-css-assets-webpack-plugin');

// 设置nodejs的环境变量
// process.env.NODE_ENV = 'development';
module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      {
        test:/\.css$/,
        use:[
          MiniCssExtractPlugin.loader,
          'css-loader',
          {
            loader:'postcss-loader',
            options:{
              ident:'postcss',
              plugins:()=>{
                require('postcss-preset-env')()
              }
            }
          }
        ]
      },
    ]
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    }),
    new MiniCssExtractPlugin({
      filename:'css/built.css'
    }),
    // 压缩css的插件
    new OptimizeCssAssetWebpackPlugin()
  ],
  mode:'production'
}
```
+ 运行指令
```bash
webpack
```

------
#### 7.2.4.4 js语法检查
+ 下载安装包
```bash
npm install --S -D eslint-loader  eslint  eslint-config-airbnb-base  eslint-plugin-import
```
+ 修改配置文件
```js
  const { resolve } = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');
  const OptimizeCssAssetWebpackPlugin = require('optimize-css-assets-webpack-plugin');

  // 设置nodejs的环境变量
  // process.env.NODE_ENV = 'development';
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'built.js',
      path: resolve(__dirname, 'build')
    },
    module: {
      rules: [
        {
          test: /\.js$/,
          // 排除检查node_modules文件夹
          excluede: /node_modules/,
          loader: 'eslint-loader',
          options: {
            // 自动修复eslint的错误
            fix: true
          }
        },
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html'
      }),
      new MiniCssExtractPlugin({
        filename: 'css/built.css'
      }),
      // 压缩css的插件
      new OptimizeCssAssetWebpackPlugin()
    ],
    mode: 'production'
  }
```
+ 修改`package.json' 。避免检查第三方库。
```json
"eslintConfig":{
	"extends":"airbnb-base",
	"env":{
		"browser":true
	}
}
```
+ 设置不被eslint检查的代码
```js
// index.js 某一行代码不做eslint检查
//eslint-disable-next-line
console.log(111);
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.4.5 js兼容性处理
+ 下载依赖安装包
```bash
npm instal -D babel-loader @babel/core @babel/preset-env @babel/polyfill core-js
```
> 基本js兼容性处理: @babel/preset-env , 如：不能转换promise
> 全部js兼容性处理: @babel/polyfill  ， 需要引入所有兼容性代码，体积太大
> 按需做兼容性处理：按需加载，基于core-js

+ 修改配置文件
```js
  const { resolve } = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'built.js',
      path: resolve(__dirname, 'build')
    },
    module: {
      rules: [
        {
          test: /\.js$/,
          // 排除检查node_modules文件夹
          excluede: /node_modules/,
          loader: 'babel-loader',
          options: {
            // 预设：指示babel做怎么样的兼容性处理(基于core-js实现按需加载)
            presets: [
              [
                '@babel/preset-env',
                {
                  // 【按需加载】
                  useBuiltIns: 'usage',
                  // 【指定core-js 版本】
                  corejs: {
                    version: 3
                  },
                  // 【兼容性做到哪个版本的浏览器】
                  target: {
                    chrome: '60',
                    firefox: '60',
                    ie: '9',
                    safari: '10',
                    edge: '17'
                  }
                }
              ]
            ]
          }
        },
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html'
      }),
      new MiniCssExtractPlugin({
        filename: 'css/built.css'
      }),
      // 压缩css的插件
      new OptimizeCssAssetWebpackPlugin()
    ],
    mode: 'production'
  }
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.4.6 js压缩
+ 生产环境下会自动压缩js代码。
+ 修改配置文件
```js
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html'
    })
  ],
  // 生产环境下自动压缩js代码
  mode:'production'
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.4.7 HTML压缩
+ 借助html-webpack-plugin，配置`html-webpack-plugin`
+ 修改配置文件
```js
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  entry:'./src/index.js',
  output:{
    filename:'built.js',
    path:resolve(__dirname,'build')
  },
  plugins:[
    new HtmlWebpackPlugin({
      template:'./src/index.html',
      // 压缩html代码
      minify:{
        // 移除空格
        collapseWhitespace:true,
        // 移除注释
        removeComments:true
      }
    })
  ],
  // 生产环境下自动压缩js代码
  mode:'production'
}
```
+ 运行指令
```bash
webpack
```
------
#### 7.2.4.8 完整的生产环境配置✅
```js
  const { resolve } = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');
  const OptimizeCssAssetWebpackPlugin = require('optimize-css-assets-webpack-plugin');


  process.env.NODE_ENV = 'development';

  const commonCssLoader = [
    MiniCssExtractPlugin.loader,
    'css-loader',
    {
      // 在package.json中定义browerslist
      loader: 'postcss-loader',
      options: {
        ident: 'postcss',
        plugins: () => [require('postcss-preset-env')()]
      }
    }
  ];

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'built.js',
      path: resolve(__dirname, 'build')
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [...commonCssLoader]
        },
        {
          test: /\.less$/,
          use: [...commonCssLoader, 'less-loader']
        },
        // 当一个文件需要被多个loader处理，那么一定要指定loader执行的先后顺序
        // ****js语法检查****
        {
          test: /\.js$/,
          excluede: /node_modules/,
          // 【当前loader优先执行】
          loader: 'eslint-loader',
          options: {
            fix: true
          }
        },
        // *****js兼容性处理*****
        {
          test: /\.js$/,
          excluede: /node_modules/,
          loader: 'babel-loader',
          options: {
            presets: [
              '@babel/preset-env',
              {
                useBuiltIns: 'usage',
                corejs: {
                  version: 3
                },
                target: {
                  chrome: '60',
                  firefox: '50'
                }
              }
            ]
          }
        },
        {
          test: /\.(jpg|png|gif)$/,
          loader: 'url-loader',
          options: {
            limit: 8 * 1024,
            name: '[hash:10].[ext]',
            outputPath: 'imgs'
          }
        },
        {
          test: /\.html$/,
          loader: 'html-loader'
        },
        {
          exclude: /\.(css|js|html|less|jpg|png|gif)$/,
          loader: 'file-loader',
          options: {
            outputPath: 'media'
          }
        },
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html',
        minify: {
          collaspseWhitespace: true,
          removeComments: true
        }
      }),
      new MiniCssExtractPlugin({
        filename: 'css/built.css'
      }),
      // 压缩css的插件
      new OptimizeCssAssetWebpackPlugin()
    ],
    mode: 'production'
  }
```

------
### 7.2.5 优化配置

+ 开发环境性能优化: 
	- 优化打包构建速度;(HMR)
	- 优化代码调试;(SourceMap)

+ 生产环境性能优化：
	- 优化打包构建速度
	- 优化代码运行的性能

#### 7.2.5.1 HMR(开发环境)
+  **HMR**（hot module replacement), 即**模块热替换**：一个模块发生变化，只会重新打包这个模块，而不是打包所有，极大地提升代码的构建速度。
+  HMR基于devServer，只能使用在开发环境。
	- css样式文件：可以使用HMR；
	- js文件：默认无法使用HMR，需要修改js代码，添加支持HMR的代码；
	```js
	// !只能在处理非入口js文件时实现HMR
if (module.hot) {
    // module.hot 为true，说明开启了HMR功能，让HMR功能生效
    module.hot.accept('./print.js', function () {
      // 监听print.js文件的变化，一旦发生变化，其他模块默认不会重新打包构建。
      print()
    })
  }
	```
	- html文件: 默认无法使用HMR , 不需要做HMR功能。
	
+ 修改配置文件
```js
  const { resolve } = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  module.exports = {
    // 入口文件要把html文件引入，可以解决html不能热更新的问题
    entry: ['./src/js/index.js', './src/index.html'],
    output: {
      filename: 'js/built.js',
      path: resolve(__dirname, 'build');
    },
    module: {
      rules: [
        //......
      ]
    },
    plugins: [
      // plugins 的配置
      new HtmlWebpackPlugin({
        template: './src/index.html'
      })
    ],
    mode: 'development',
    devServer: {
      contentBase: resolve(__dirname, 'build'),
      compress: true,
      port: 3000,
      open: true,
      // 开启HMR功能，
      // 当修改了webpack配置，新配置想要生效，必须重新启动webpack服务
      hot: true
    }
  }
```
+ 运行指令
```bash
npx  webpack-dev-server
```
------
#### 7.2.5.2 source-map
+ **source-map**提供了从源代码到构建后代码的映射技术。如果构建后的代码出错，那么可以通过映射关系追踪到源代码的错误。
	- 开发环境，要考虑调试友好，一般使用 `eval-source-map`。
	- 生产环境，不使用内联，一般使用`source-map`或`cheap-module-source-map`。如果要全部隐藏打包后的代码和源代码，可以使用`nosources-source-map` ，只隐藏源代码，显示构建后的代码的错误，可以使用`hidden-source-map`。
+ 几种source-map的映射模式：
  \[inline -|hidden -|eval -] \[nosources -] [cheap-[module-]] source-map
	- source-map : 在外部生成`.map`文件。可以看到错误代码的准确信息和源代码的错误位置。
	- inline-source-map: source-map 代码内联，只生成一个内联的source-map代码。可以看到错误代码的准确信息和源代码的错误位置。
	- hidden-source-map：在外部生成`.map`文件。不能追踪源代码错误，只能提示到构建后代码的错误位置。
	- eval-source-map：source-map 代码内联，每个文件都声称对应的source-map代码，都在eval中。可以看到错误代码的准确信息和源代码的错误位置。
	- nosources-source-map: 在外部生成`.map`文件。可以看到错误代码的准确信息，但是没有任何源代码信息。
	- cheap-source-map: 在外部生成`.map`文件。可以看到错误代码的准确信息，和源代码的错误位置，只能精确到行。
	- cheap-module-source-map: 在外部生成`.map`文件。可以看到错误代码的准确信息，和源代码的错误位置。module会将loader的source map 加入。

<div align="center">
	<img src="./0_pictures/sourcemap.png" alt="img20" style="zoom:65%;" />
</div>

+  修改配置文件

```js
//...代码同上
mode: 'development',
    devServer: {
    contentBase: resolve(__dirname, 'build'),
      compress: true,
        port: 3000,
          open: true,
            hot: true
  },
  // 开启source-map
  devtool: 'source-map'
```

------
#### 7.2.5.3 oneOf优化配置
+ 针对单个源文件，module.rules 里面的规则都会检测一遍，这样会比较浪费性能。可以把只需要检测处理一次的loader 放到rules 数组里面,这样只要成功匹配一个loader并完成检测后，就不再检测其余loader。
+ oneOf 用来优化的打包速度。
```js
// ...生产环境代码省略
module.exports = {
	entry: './src/js/index.js',
  output: {
      filename: 'js/built.js',
      path: resolve(__dirname, 'build');
  },
	module:{
		rulse:[
			{
				test:/\.js$/,
				exclude:/node_modules/,
				enforce:'pre',
				loader:'eslint-loader',
				options:{
				  fix:true
			  }
		  },
      {
        // oneOf里面的loader 只会过一次
        oneOf:[
          {
            test:/\.css$/,
            use:[...commonCssLoader]
          },
          {
            test:/\.less$/,
            use:[...commonCssLoader,'less-loader']
          },
          // ......
        ]
      }
		]
	}
  // ...
}
```
------
#### 7.2.5.4 缓存
+ 同HMR功能类似，为了避免生产环境下局部模块代码的改动引发全部代码的重新构建，需要开启**缓存**。生产环境下不可以使用HMR。
	- `babel缓存`:针对js文件，直接配置`cateDirectory：true`
	- 文件资源缓存：通过**hash值**给构建代码重命名，避免部分缓存失效。
  + \[hash:10]: 每次webpack构建时都会生成一个唯一的hash值, 但是js和css会使用同一hash值，这时，如果只改动一个文件，会导致所有缓存都失效。
	  + \[chunkhash:10]: 根据chunk生成的hash值，如果打包来源于同一个chunk，那么hash值时一样的，但是此时js和css的hash值还是一样的，因为css是在js中被引入的，同属一个chunk。
	  + \[contenthash:10]: ✅根据文件的内容来生成hash值，不同的文件hash值一定不一样。
	
+ 修改配置文件
```js
// 生产环境代码省略
  module.exports = {
    entry: './src/js/index.js',
    output: {
       // 【产出代码重命名加十位哈希值】
       filename: 'js/built.[contenthash:10].js',
       path: resolve(__dirname, 'build');
    },
    module: {
      rulse: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          enforce: 'pre',
          loader: 'eslint-loader',
          options: {
            fix: true
          }
        },
        {
          // oneOf里面的loader 只会过一次
          oneOf: [
            {
              test: /\.css$/,
              use: [...commonCssLoader]
            },
            {
              test: /\.less$/,
              use: [...commonCssLoader, 'less-loader']
            },
            // *****js兼容性处理*****
            {
              test: /\.js$/,
              excluede: /node_modules/,
              loader: 'babel-loader',
              options: {
                presets: [
                  [
                    '@babel/preset-env',
                    {
                      useBuiltIns: 'usage',
                      corejs: {
                        version: 3
                      },
                      target: {
                        chrome: '60',
                        firefox: '50'
                      }
                    }
                  ]
                ]
              }
            },
          ]
        }
      ]
    },
     plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html',
        minify: {
          collaspseWhitespace: true,
          removeComments: true
        }
      }),
      new MiniCssExtractPlugin({
        // 【产出css重命名十位哈希值】
        filename: 'css/built.[contenthash:10].css'
      }),
      new OptimizeCssAssetWebpackPlugin()
    ],
    // 【production】环境自动开启tree shaking
    mode: 'production'
  }
```

------
#### 7.2.5.5 tree shaking
+ **tree shaking**, 可以去除无用的代码，减小构建代码的体积。
+ 满足使用前提，tree shaking功能自动开启。
	- 使用ES6模块化
	- 开启production环境

+ 配置文件如7.2.5.4所示。
+ 修改package.json
```json
// 所有代码都没有副作用，都可以进行tree shaking 。 可能会把css / @babel、polyfill文件删掉。
//  "sideEffects":false

//  让css等文件不经过tree shaking处理
  "sideEffects":["*.css","*.less"]
```
------
#### 7.2.5.6 code split
+ 代码分割技术主要针对js代码。
+ Demo1: 多入口`entry`对象. 
```js
module.exports = {
  entry:{
    // 一个入口对应输出一个bundle
    index:'./src/js/index.js',
    test:'./src/js/test.js'
  },
  output:{
    //避免打包出来的多个js文件命名冲突，使用[name]: 取文件名
    filename: 'js/[name].[contenthash:10].js',
    path:resolve(__dirname,'build')
  },
  plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html',
        minify: {
          collaspseWhitespace: true,
          removeComments: true
        }
      }),
  ]
}
```
+ Demo2 : `optimization`， 将`node_modules`中的 代码单独打包成一个chunk输出。自动分析多入口chunk中，有没有公共的文件，如果有会打包成单独一个chunk。
```js

module.exports = {
  entry:{
    // 一个入口对应输出一个bundle
      // index 代码引入jquery
    index:'./src/js/index.js',
      // test 代码引入jquery
    test:'./src/js/test.js'
  },
  output:{
    //避免打包出来的多个js文件命名冲突，使用[name]: 取文件名
    filename: 'js/[name].[contenthash:10].js',
    path:resolve(__dirname,'build')
  },
  plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html',
        minify: {
          collaspseWhitespace: true,
          removeComments: true
        }
      }),
  ],
  // jquery 实际只被打包了一次
  optimization:{
    splitChunks: {
      chunks:'all'
    }
  }
}
```

+ Demo3 : 通过js代码，让某个文件被单独打包成一个chunk。
```js
// index.js
// es10 的import 语法
// webpackChunkName固定打包名字
import (/*webpackChunkName:'test'*/'./text.js')
  .then(({mul,count})=>{
    // eslint-disable-next-line
    console.log(mul(2*5))
  })
  .catch(()=>{
    // eslint-disable-next-line
    console.log('加载失败')
  })
// 配置文件同Demo2
```

------
#### 7.2.5.7 懒加载和预加载。
+ **懒加载**：触发事件再加载；**预加载**：浏览器空闲后偷偷加载，但有兼容性问题。

+ 在js代码内部实现懒加载。
```js
// index.js
console.log('index.js文件被加载了');
document.getElmentById('btn').onclick = function(){
  // 懒加载~
  // 预加载 prefetch: 会在使用之前，提前加载js文件
  import(/*webpackChunkName:'test',webpackPrefetch:true*/'./test')
  .then(({mul})=>{
    console.log(mul(4,5))
  })
}
```
------
#### 7.2.5.8 离线可访问技术

+ PWA，(Progressive Web App) 渐进式网络开发应用程序，也即离线可访问技术。加载过一次后，断网后再刷新也能保留一定的页面资源。
+ PWA依托库`workbox`.
+ 下载安装包插件
```bash
npm install -D workbox-webpack-plugin
```
+ 修改配置文件
```js
const {resolve} = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
const WorkboxWebpackPlugin = require('workbox-webpack-plugin');

//...
module.exports = {
  entry:'./src/js/index.js',
  output:{
    filename:'js/built.[contenthash:10].js',
    path:resolve(__dirname,'build')
  },
  module:{
    rules:[
      //...
    ]
  },
  plugins:[
    //...
    new WorkboxWebpackPlugin.GenerateSW({
      // 1. 帮助serviceWorker快速启动
      // 2. 删除旧的serviceWorker
      //  生成一个serviceworker配置文件
      clientsClaim:true,
      skipWaiting:truW
    })
  ],
  mode:'production',
  devtool:'source-map'
}
```
+ js 中注册serviceWorker,处理兼容性问题
```js
// index.js
if('serviceWorker' in navigator){
  window.addEventListener('load',()=>{
    navigator.serviceWorker
      .register('/service-worker.js')
      .then(()=>{
       //eslint-disable-next-line
      console.log('注册成功')
    })
      .catch(()=>{
        //eslint-disable-next-line
      console.log('sw注册失败')
    })
  })
}
```
+ eslint  不认识window、 navigator全局变量，需要修改`package.json`中的eslintConfig配置。Note：serviceWorker代码必须运行在服务器上。
```json
// package.json
//...
"eslintConfig":{
  "extends":"airbnb-base",
  "env":{
    // 支持浏览器端的全局变量
    "browser":true
  }
},
"sideEffects":[
 "*.css"
]
```
------
#### 7.2.5.9 多进程打包

+ 使用`thread-loader`对`babel`使用多进程打包.
+ 开启多进程打包有利有弊，进程启动大概为600ms，进程通信也有开销，只有工作消耗时间比较长，才需要多进程打包。
+ 下载安装包`thread-loader`
```bash
npm install -D thread-loader
```
+ 修改配置文件
```js
  const { resolve } = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const MiniCssExtractPlugin = require('mini-css-extract-plugin');
  const OptimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin');
  const WorkboxWebpackPlugin = require('workbox-webpack-plugin');
  process.env.NODE_ENV = 'development';
  const commonCssLoader = [
    MiniCssExtractPlugin.loader,
    'css-loader',
    {
      // 在package.json中定义browerslist
      loader: 'postcss-loader',
      options: {
        ident: 'postcss',
        plugins: () => [require('postcss-preset-env')()]
      }
    }
  ];

  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'built.js',
      path: resolve(__dirname, 'build')
    },
    module: {
      rules: [
        {
          test: /\.css$/,
          use: [...commonCssLoader]
        },
        {
          test: /\.less$/,
          use: [...commonCssLoader, 'less-loader']
        },
        {
          /* js语法检查 */
          // 在package.json中eslintConfig --> airbnb
          test: /\.js$/,
          exclude: /node_modules/,
          // 优先执行
          enforce: 'pre',
          loader: 'eslint-loader',
          options: {
            fix: true,
          },
        },
        {
          test: /\.js$/,
          excluede: /node_modules/,
          use: [
            {
              loader: 'thread-laoder',
              options: {
                workers: 2
              }
            },
            {
              loader: 'babel-loader',
              options: {
                presets: [
                  [
                    '@babel/preset-env',
                    {
                      useBuiltIns: 'usage',
                      corejs: {
                        version: 3
                      },
                      target: {
                        chrome: '60',
                        firefox: '50'
                      }
                    }
                  ]
                ],
                cacheDirectory: true
              }
            }
          ]
        },
        {
          test: /\.(jpg|png|gif)$/,
          loader: 'url-loader',
          options: {
            limit: 8 * 1024,
            name: '[hash:10].[ext]',
            outputPath: 'imgs'
          }
        },
        {
          test: /\.html$/,
          loader: 'html-loader'
        },
        {
          exclude: /\.(css|js|html|less|jpg|png|gif)$/,
          loader: 'file-loader',
          options: {
            outputPath: 'media'
          }
        },
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html',
        minify: {
          collaspseWhitespace: true,
          removeComments: true
        }
      }),
      new MiniCssExtractPlugin({
        filename: 'css/built.css'
      }),
      // 压缩css的插件
      new OptimizeCssAssetWebpackPlugin(),
      new WorkboxWebpackPlugin.GenerateSW({
        clientsClaim: true,
        skipWaiting: true
      })
    ],
    mode: 'production',
    // ...
  }
```
------
#### 7.2.5.10 externals 
+ 配置externals，可以避免某些包被打包构建，比如通过CDN引入的jquery

+ 修改配置文件
```js
  const { resolve } = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  
  module.exports = {
    entry: './src/index.js',
    output: {
      filename: 'built.js',
      path: resolve(__dirname, 'build')
    },
    module: {
      rules: [
        //...
      ]
    },
    plugins: [
      new HtmlWebpackPlugin({
        template: './src/index.html'
      }),
    ],
    mode: 'production',
    // 拒绝jQuery被打包进来
    externals:{
      jquery:'jQuery'
    }
  }
```
------
#### 7.2.5.11 dll
+ 使用dll技术（Dynamic Link Library)，对某些库（第三方库：jquery，react，vue...)进行单独打包，当运行`webpack`命令时，会默认查找`webpack.config.js`文件。我们需要配置运行`webpack.dll.js`文件，命令`webpack --config webpack.dll.js`。

+ 文件关系

  <div align="center">
  	<img src="./0_pictures/webpack-dll.png" alt="img20" style="zoom:65%;" />
  </div>

+ 修改配置文件`webpack.dll.js` 。 这个文件定义需要单独打包的库。
```js
const { resolve } = require('path')
const webpack = require('webpack')

module.exports = {
  entry: {
    // 最终打包生成的[name] -->jquery
    //         ['jquery'] --> 要打包的库是jquery
    jquery: ['jquery'],
  },
  output: {
    filename: '[name].js',
    path: resolve(__dirname, 'dll'),
    library: '[name]_[hash]', // 打包的库里面向外暴露出去的内容叫什么名字
  },
  plugins: [
    //  打包生成一个manifest.json文件 -->提供和jquery映射
    new webpack.DllPlugin({
      name: '[name]_[hash]', // 映射库的暴露的内容名称
      path: resolve(__dirname, 'dll/manifest.json'), // 输出文件路径
    }),
  ],
  mode: 'production',
}
```
+ 安装插件`add-asset-html-webpack-plugin`
```bash
npm i -D add-asset-html-webpack-plugin
```
+ 修改配置文件 `webpack.config.js`
```js
const { resolve } = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');
const AddAssetHtmlWebpackPlugin = require('add-asset-html-webpack-plugin');
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'built.js',
    path: resolve(__dirname, 'build'),
  },
  module: {
    rules: [
      //loader的配置
    ],
  },
  plugins: [
    //plugins的配置
    new HtmlWebpackPlugin({
      template: './src/index.html',
    }),
    // 告诉webpack哪些库不参与打包，同时使用时的名称也得变
    new webpack.DllReferencePlugin({
      manifest: resolve(__dirname, 'dll/manifest.json'),
    }),
    // 将某个文件(jquery)打包输出出去，并在html中自动引入该资源
    new AddAssetHtmlWebpackPlugin({
      filepath: resolve(__dirname, 'dll/jquery.js'),
    }),
  ],
  mode: 'development',
};
```
------
### 7.2.6 配置详情

待补充

------

### 7.2.7 Webpack5

待补充

------
### 7.2.7 面试题

#### 7.2.7.1 谈谈你对webpack的理解

+ Webpack 是一个模块打包工具，可以使用Webpack管理模块依赖，并编译出各模块所需要的静态文件。它能够很好地管理、打包Web开发中所用到的HTML、Javascript、CSS以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack有对应的模块加载器。Webpack模块打包器会分析模块间的依赖关系，最后生成了优化且合并后的静态资源。

+ Webpack 具有五个核心的概念，分别是 Entry（入口）、Output（输出）、loader 、 Plugins（插件）和mode（模式）。

+ Entry 是 webpack 的入口起点，它指示 webpack 应该从哪个模块开始着手，来作为其构建内部依赖图的开始。

+ Output 属性告诉 webpack 在哪里输出它所创建的打包文件，也可指定打包文件的名称，默认位置为 ./dist。

+ loader 可以理解为 webpack 的编译器，它使得 webpack 可以处理一些非 JavaScript 文件。在对 loader 进行配置的时候，test 属性，标志有哪些后缀的文件应该被处理，是一个正则表达式。use 属性，指定 test 类型的文件应该使用哪个 loader 进行预处理。常用的 loader 有 css-loader、style-loader 等。

+ 插件可以用于执行范围更广的任务，包括打包、优化、压缩、搭建服务器等等，要使用一个插件，一般是先使用 npm 包管理器进行安装，然后在配置文件中引入，最后将其实例化后传递给 plugins 数组属性。
+ mode 可以设定当前环境是生产环境(production)还是开发环境(development)。
------
#### 7.2.7.2 说说webpack的作用？
+ **模块打包**。可以将不同模块的文件打包整合在一起，并且保证它们之间的引用正确，执行有序。利用打包我们就可以在开发的时候根据我们自己的业务自由划分文件模块，保证项目结构的清晰和可读性。
+ **编译兼容**。在前端的“上古时期”，手写一堆浏览器兼容代码一直是令前端工程师头皮发麻的事情，而在今天这个问题被大大的弱化了，通过webpack的`Loader`机制，不仅仅可以帮助我们对代码做polyfill，还可以编译转换诸如.less, .vue, .jsx这类在浏览器无法识别的格式文件，让我们在开发的时候可以使用新特性和新语法做开发，提高开发效率。
+ **能力扩展**。通过webpack的`Plugin`机制，我们在实现模块化打包和编译兼容的基础上，可以进一步实现诸如按需加载，代码压缩等一系列功能，帮助我们进一步提高自动化程度，工程效率以及打包输出的质量。

> 更多内容参考: [《当面试官问Webpack的时候他想知道什么》](https://juejin.cn/post/6943468761575849992)

# 8.Vue
## 8.1 MVVM与双向绑定数据
### 8.1.1 什么是MVVM？比之MVC有什么区别？
+ MVC、MVP 和 MVVM 是三种常见的软件架构设计模式，主要通过分离关注点的方式来组织代码结构，优化我们的开发效率。
+ MVC 通过分离 Model、View 和 Controller 的方式来组织代码结构。其中 View 负责页面的显示逻辑，Model 负责存储页面的业务数据，以及对相应数据的操作。并且 View 和 Model 应用了观察者模式，当 Model 层发生改变的时候它会通知有关 View 层更新页面。Controller 层是 View 层和 Model 层的纽带，它主要负责用户与应用的响应操作，当用户与页面产生交互的时候，Controller 中的事件触发器就开始工作了，通过调用 Model 层，来完成对 Model 的修改，然后 Model 层再去通知 View 层更新。
+ MVP 模式与 MVC 唯一不同的在于 Presenter 和 Controller。在 MVC 模式中我们使用观察者模式，来实现当 Model 层数据发生变化的时候，通知 View 层的更新。这样 View 层和 Model 层耦合在一起，当项目逻辑变得复杂的时候，可能会造成代码的混乱，并且可能会对代码的复用性造成一些问题。MVP 的模式通过使用 Presenter 来实现对 View 层和 Model 层的解耦。MVC 中的Controller 只知道 Model 的接口，因此它没有办法控制 View 层的更新，MVP 模式中，View 层的接口暴露给了 Presenter 因此我们可以在 Presenter 中将 Model 的变化和 View 的变化绑定在一起，以此来实现 View 和 Model 的同步更新。这样就实现了对 View 和 Model 的解耦，Presenter 还包含了其他的响应逻辑。
+ MVVM 模式中的 VM，指的是 ViewModel，它和 MVP 的思想其实是相同的，不过它通过双向的数据绑定，将 View 和 Model 的同步更新给自动化了。当 Model 发生变化的时候，ViewModel 就会自动更新；ViewModel 变化了，View 也会更新。这样就将 Presenter 中的工作给自动化了。我了解过一点双向数据绑定的原理，比如 vue 是通过使用数据劫持和发布订阅者模式来实现的这一功能。

> 详细资料可以参考：[《浅析前端开发中的 MVC/MVP/MVVM 模式》](https://juejin.im/post/593021272f301e0058273468)、[《MVC，MVP 和 MVVM 的图示》](http://www.ruanyifeng.com/blog/2015/02/mvcmvp_mvvm.html)、[《MVVM》](https://juejin.im/book/5bdc715fe51d454e755f75ef/section/5bdc72e6e51d45054f664dbf)、[《一篇文章了解架构模式：MVC/MVP/MVVM》](https://segmentfault.com/a/1190000015310674)
------
### 8.1.2 说说对于MVVM的理解
+ **MVVM**是`Model-View-ViewModel`，是一种软件架构的设计模式或思想。
	- Model代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑。
	- View代表UI组件，它负责将数据模型转化成UI展现出来。
	- ViewModel 数据模型的改变和控制视图行为、处理用户交互，简单理解它就是一个同步View和Model的对象，连接Model和View。
> 在MVVM架构下，View和Model之间并没有直接的联系，而是通过ViewModel进行交互，Model和ViewModel之间的交互是双向的，因此View数据的变化会同步到Model中,而Model数据的变化也会立即反应到View上。
> ViewModel通过双向数据绑定把View层和Model层连接了起来，而View和Model之间的同步工作是完全自动的，无需人为干涉，因此**开发者只需关注业务逻辑，不需要手动操作DOM，不需要关注数据状态的同步问题，复杂的数据状态维护完全由MVVM来统一管理**。
>
> 详细资料可以参考：[MVVM是什么?](https://www.jianshu.com/p/6aeeecd64dcf)
------
### 8.1.3 Vue的响应式原理？
+ 当你把一个普通的 JavaScript 对象传入 Vue 实例作为 `data` 选项，Vue 将遍历此对象所有的 property，并使用 `Object.defineProperty` 把这些 property 全部转为 [getter/setter](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Working_with_Objects#定义_getters_与_setters)。用户看不到getter/setter，但是在内部它们让Vue能够追踪依赖，在property被访问和被修改时通知变化。每个组件实例都对应一个watcher实例，它会在组件渲染的过程中把“接触”过的数据property记录为依赖。之后当依赖项的setter触发时，会通知watcher，从而使它关联的组件重新渲染。

> data对象里面的数据会被变成getter和setter函数，然后watcher会监听这些变化。当data里面的数据被访问的时候，会触发getter函数；当data里面的数据被修改的时候会触发setter函数。修改数据，并进行DOM树比较，重新渲染模板。

+ Vue的双向数据绑定将MVVM作为数据绑定的入口，整合Observer，Compile和Watcher三者，通过Observer来监听自己的model的数据变化，通过Compile来解析编译模板指令（vue中用来解析`{{}}`），最终利用watcher搭起observer和Compile之间的通信桥梁，达到数据变化 → 试图更新 ； 视图交互变化 → 数据model变更的双向绑定效果。

<div align="center">
  <img src="./0_pictures/activityVue.png" alt="lifeT" />
</div>
> 详细内容参考：[《深入响应式原理》](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%A6%82%E4%BD%95%E8%BF%BD%E8%B8%AA%E5%8F%98%E5%8C%96)

--------
### 8.1.4 请简单实现双向数据绑定mvvm？
```html
<body>
<input id="input">

<script>
 const data = {};
 const input = document.getElementById("input");
 // Vue2 响应式底层依赖于Object.defineProperty()函数
  Object.defineProperty(data,'text',{
   set(value){
     input.value = value;
     this.value = value;
   },
   get(value){
     // this 指向data.text
     return this.value;
   }
 });
  // 输入框内容改变的事件
 input.onchange = function(e){
   data.text = e.target.value;
   console.log(data.text)
 };
  // 1.输入框内容改变，控制台数据改变
  // 2.控制台改变data.text ,输入框数据改变
</script>
</body>
```
------
### 8.1.5 Object.defineProperty()函数？
+ **Object.defineProperty()**方法直接在一个对象上定义新的属性或修改一个对象的现有的属性，并返回该对象。这个函数一共有三个参数，第一个参数是`需要定义属性的对象`，第二个参数是`需要定义的属性`，第三个是`该属性描述符`。对象里目前存在的属性描述符有两种主要形式：*数据描述符*和*存取描述符*。数据描述符里的`数据属性`和存取描述符里的`访问器属性`不能混合使用。

  - 对于对象的`数据描述符`（数据属性）, 可以由四个特性来组成其描述符对象，分别是：` value` 表示属性的值，`writable` 表示属性是否可以被修改，`enumerable` 表示属性是否可枚举，`configurable` 表示属性是否可以通过delete删除并重新定义。

    ```js
    let person = {}
    Object.defineProperty(person,"name",{
      writable:false,
      value:"Nicholas"
    });
    console.log(person.name);//Nicholas
    person.name="Greg";
    console.log(person.name);//Nicholas //不可修改
    ```

  - 对于对象的`存取描述符`（访问器属性）, 可以由四个特性来组成其描述符对象，分别是: `get` 属性获取函数，`set` 属性设置函数，`enumerable` 属性是否可枚举，`configurable` 表示属性是否可以通过delete删除并重新定义。

    + 访问器属性是不能直接定义的，必须要使用`Object.defineProperty()`函数来定义。

    + 获取函数`get`和设置函数`set`不一定都要定义。只定义`get`意味着属性是只读的，只定义`set`意味着属性是不能读取的，非严格模式读取会返回undefined，严格模式会抛出错误。

    ```js
    let book = {
      year_:2017,
      edition:1
    };
    // 定义一个访问器属性“year”
    Object.defineProperty(book,"year",{
      get(){
        return this.year_;
      },
      set(newValue){
        if(newValue>2017){
          this.year_ = newValue;
          this.edition + = newValue -2017;
        }
      }
    });
    ```
> 详细资料可以参考：[《Object.defineProperty()》](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

------
### 8.1.6  🔥使用Obeject.defineProperty()来进行数据劫持有什么缺点?（Vue2.X的响应式特点？）
+ 有一些对属性的操作，使用这种方法无法拦截，比如说**通过下标方式修改(data里面的)数组数据或者给对象新增、删除属性**，vue 内部通过**重写函数**解决了这个问题。

  - 对于已经创建的实例，Vue 不允许动态添加根级别的响应式 property。但是，可以使用 `Vue.set(object, propertyName, value)` 方法向嵌套对象或数组**添加**响应式 property; `Vue.delete(object,propertyName)`方法**删除**属性并响应。
  ```js
  Vue.set(vm.someObject,'b',2);
  Vue.delete(vm.someObject,'a');
  ```
  - 还可以使用 `vm.$set`或`vm.$delete` 实例方法，这也是全局 Vue.set 方法的别名。
  ```js
  // 修改
  this.$set(this.someObject,'b',2);
  // 删除
  this.$delete(this.someObject,'a');
  ```
  - 对于数组的修改，[推荐使用api](#ArrayMethods)。比如，splice，push，shift，unshift等。这些api方法Vue已经做了响应式处理。
  > 参考资料:[官方Vue：检测变换的注意事项](https://cn.vuejs.org/v2/guide/reactivity.html#检测变化的注意事项)

+ 在 Vue3.0 中已经不使用这种方式了，而是通过使用 Proxy 对对象进行代理，从而实现数据劫持。使用 Proxy 的好处是它可以**完美的监听到任何方式的数据改变**，唯一的缺点是兼容性的问题，因为这是 ES6 的语法。

```js
// vue2 底层响应式
Object.defineProperty(data,'count',{
  get(){},
  set(){},
})

// vue3 底层响应式
new Proxy(data,{
  get(key){},
  set(key,value){}
})
```
------
## 8.2 Vue框架的特点
### 8.2.1 什么是Virtual DOM? 为什么要使用Virtual DOM？

+ 用Javascript对象结构表示DOM数的结构，然后用这个树构建一个真正的DOM树，插到文档中。

+ 当页面的状态发生改变，我们需要对页面的 DOM 的结构进行调整的时候，我们首先根据变更的状态，重新构建起一棵对象树，然后将这棵新的对象树和旧的对象树进行比较，记录下两棵树的的差异。

+ 最后将记录的有差异的地方应用到真正的 DOM 树中去，这样视图就更新了。

+ Virtual DOM 这种方法对于我们需要有大量的 DOM 操作的时候，能够很好的提高我们的操作效率，通过在操作前确定需要做的最小修改，尽可能的减少 DOM 操作带来的重流和重绘的影响。其实 Virtual DOM 并不一定比我们真实的操作 DOM 要快，这种方法的目的是为了提高我们开发时的可维护性，在任意的情况下，都能保证一个尽量小的性能消耗去进行操作。
> Virtual DOM的概念有很多解释，从我的理解来看，主要是三个方面，分别是：一个对象，两个前提，三个步骤。**一个对象**指的是Virtual DOM是一个基本的JavaScript对象，也是整个Virtual DOM树的基本。**两个前提**分别是JavaScript很快和直接操作DOM很慢，这是Virtual DOM得以实现的两个基本前提。得益于V8引擎的出现，让JavaScript可以高效地运行，在性能上有了极大的提高。直接操作DOM的低效和JavaScript的高效相对比，为Virtual DOM的产生提供了大前提。**三个步骤**指的是Virtual DOM的三个重要步骤，分别是：生成Virtual DOM树、对比两棵树的差异、更新视图。

> 详细资料可以参考：[《Virtual DOM》](https://juejin.im/book/5bdc715fe51d454e755f75ef/section/5bdc72e6e51d45054f664dbf)、[《理解 Virtual DOM》](https://github.com/y8n/blog/issues/5)、[《深度剖析：如何实现一个 Virtual DOM 算法》](https://github.com/livoras/blog/issues/13)、[《网上都说操作真实 DOM 慢，但测试结果却比 React 更快，为什么？》](https://www.zhihu.com/question/31809713/answer/53544875)
------
### 8.2.2 vue框架的两大特点是什么？
+ 数据驱动（双向数据绑定）;
+ 组件化开发.
------

## 8.3 Vue的生命周期

### 8.3.1 Vue生命周期的概念

  - 每个Vue实例在被创建时，都要经过一系列的初始化过程，例如，需要设置数据监听、编译模板、将实例挂载到DOM并在数据变化时，更新DOM等。同时在这个过程中，也会运行一些叫做`生命周期钩子`的函数，这给了用户在不同阶段添加自己的代码的机会。

### 8.3.2 生命周期钩子函数

  - **beforeCreate** : vue实例创建之前。可以访问this对象。vue实例的挂载元素el和数据data对象都为undefined 。此时，不能访问到实例上的data，methods、props等属性上的数据。

  - **created** 📢：vue实例创建完成。data、methods，watch，computed等属性都可以访问到。
    + 此时，常用于简单的ajax请求和**获取首屏数据**。一旦此时的ajax请求过多，就会出现长时间的白屏现象。
    
    + 此时，还没有挂载，还不能访问`this.$el`。
    
      ------
    
  - **beforeMount**: vue实例即将挂载到页面。
    
    + 初始化实例的时候，存在`el`字段或`template`字段才会进入这个生命周期。
    + 此时，即将要把内存中的HTML结构渲染到页面上，但是此时并没有真正开始渲染，所以页面上还是空白的，用户看到的是插值表达式这样的页面结构。
    
  - **mounted** 📢: vue实例挂载完成。真实的DOM节点被渲染，元素已经显示在网页上。
    + 此时，可以通过DOM API获取dom节点，$ref属性可以访问。一般会进行一些插件的初始化；也可以发起一些大量的数据请求。
    
    + 获取元素的位置，尺寸等参数的时候，就在这个阶段。
    
    + 组件中如果有子组件的话，会递归挂载子组件，只有当子组件全部挂载完毕，才会执行根组件的挂载钩子。
    
      ------
    
  - **beforeUpdate** ：响应式数据更新，即data里的数据已经更新，但是用户看到的页面上的数据还没有更新。
    
    + 此时，适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器。
    + 注意：如果一个数据更新后，并没有渲染显示出这个数据。那么这两个钩子函数不会触发。所以，<u>这两个钩子函数发生的准确时机是“影响页面展示的数据发生更新前后”</u>
    
  - **updated**: 页面完成了更新，data和页面都是最新的。
    
    + 此时，一般也会在这里进行插件初始化。避免在这时操作数据，可能陷入死循环。
    
      ------
    
  - **beforeDestroy** : vue实例即将被销毁。
    
    - 此时，vm实例的数据还可以被访问。适合移除事件监听、定时器，释放一些变量。
  - **destroyed**: vue实例被销毁。
    
    + 此时，vm实例已经不存在。

<div align="center">
  <img src="./0_pictures/vueLifeDetail.png" alt="lifeT" />
</div>

------
#### 8.3.2.1  🚀  \[面试题] 第一次页面加载会触发几个生命周期钩子？
+ 四个：`beforeCreate`、`created`、`beforeUpdate`、`updated`

------
#### 8.3.2.2 🚀  \[面试题] DOM渲染在哪个钩子就已经完成？
+ DOM渲染在`mounted`中就已经完成了。

------
#### 8.3.2.3 🚀  \[面试题] 当首次页面加载后，再改变data中定义的变量时，会执行哪些钩子函数？

+ 响应式变量的值发生变化时，会执行`beforeUpdate`和`updated`钩子函数。
------
#### 8.3.2.4 🚀  \[面试题] 如果当前的组件没有被缓存，切换组件的时候，会执行哪些钩子函数？
+ 在切换组件的时候，如果当前的组件没有被缓存，会执行**新组件**的`beforeCreate`、`Created`、`beforeMount`这三个钩子函数，然后执行**当前组件**的`beforeDestroy`和`Destroyed`钩子函数，最后在执行**新组件**的`mounted`钩子函数。
------

## 8.4 Vue指令

+ 指令就是Vue给标签增加的新属性，以`v-`开头。

### 8.4.1 v-html
+ `v-html`: 将数据解析为html，更新元素的`innerHTML`。
### 8.4.2 v-text
+ `v-text`: 解析数据，更新元素的 textContent。类似于`innerText`。
### 8.4.3 🐯 v-bind
+ `v-bind`: 将一个元素的原有属性动态地绑定变量。常常简写为`:`。

+ 在将 `v-bind` 用于 class 和 style 时，Vue.js 做了专门的增强。表达式结果的类型除了字符串之外，还可以是对象或数组。

  - 绑定HTML class样式
      + 对象语法: 传给 `v-bind:class` 一个对象，以动态地切换 class。
      ```html
    <div
       class="static"
       v-bind:class="{ active: isActive, 'text-danger': hasError}"
    ></div>
  
    <script>
     var vm = new Vue({
       data: {
          isActive: true,
          hasError: false
       }
    });
    </script>
  
    <!--结果渲染为 -->
    <div class="static active"></div>
    ```
  
     + 数组语法: 传给 `v-bind:class` 一个数组，以动态地切换 class。
     ```html
    <div :class="[activeClass, errorClass]"></div>
    
    <script>
      var vm = new Vue({
      data: {
           activeClass: 'active',
           errorClass: 'text-danger'
        }
     });
    </script>
    
    <!--结果渲染为 -->
    <div class="active text-danger"></div>
     ```
  
  - 绑定内联style样式
    + 内联style绑定同样支持数组和对象的形式。
    ```html
    <div v-bind:style="styleObject"></div>
    <script>
    var vm = new Vue({
      data: {
         styleObject: {
           color: 'red',
           fontSize: '13px'
         }
      }
    });
    </script>
    ```

    + `v-bind:style` 的数组语法可以将多个样式对象应用到同一个元素上：
    ```html
    <div v-bind:style="[baseStyles, overridingStyles]"></div>
    ```
> 详细参考：[《vue官方：class与style绑定》](https://cn.vuejs.org/v2/guide/class-and-style.html)
-------
### 8.4.4 🐯 v-if & v-show
+ `v-if` 指令用于条件性地渲染一块内容。
  - 可以使用 `v-else` 指令来表示 `v-if` 的“else 块”,`v-else`元素必须紧跟在带 `v-if` 或者 `v-else-if` 的元素的后面，否则它将不会被识别。`v-else-if`指令同理。
  - 可以在\<template>元素上使用`v-if`条件渲染一个分组。
  - `v-if`与`v-else`形成并列的层级关系时，同样可以结合`key`，来管理可复用的元素。
- [《官方Vue:用key管理可复用的元素》](https://cn.vuejs.org/v2/guide/conditional.html#用-key-管理可复用的元素)
  
+ `v-show`指令根据条件展示元素。
  - 带有 v-show 的元素始终会被渲染并保留在 DOM 中。v-show 只是简单地切换元素的 CSS property display。

#### 8.4.4.1 🚀  \[面试题] `v-if`与`v-show` 有何区别？
  - `v-if` 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
  - `v-show` 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
  - 一般来说，`v-if`有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，**如果需要非常频繁地切换，则使用 `v-show` 较好**；如果在运行时条件很少改变，则使用 `v-if` 较好。
> 详细参考：[《官方vue：条件渲染v-if》](https://cn.vuejs.org/v2/guide/conditional.html#v-if)
--------
### 8.4.5 🐯 v-for 
+ 使用`v-for` 指令可以基于一个数组来渲染一个列表。
+ 使用`v-for` 指令遍历一个对象。
+ 📢当 Vue 正在更新使用 `v-for` 渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素，并且确保它们在每个索引位置正确渲染。为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 `key` attribute。
```vue
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
</div>
```
------
#### 8.4.5.1  🚀 \[面试题] Vue中的v-for循环，为什么需要绑定key值？

+ 使用key给数组某个元素绑定在一起，如果那个key对应的数据发生变化，直接更新对应的dom就行，不用全部更新一遍。这也是vue不推荐使用数组下标作为key的原因。例如数组删除了一个元素，那么这个元素后方元素的下标全都前移了一位，之前key对应的数据和dom就会乱了，除非重新匹配key，那就容易产生错误。如果重新匹配key，等于全部重新渲染一遍，违背了使用key来优化更新dom的初衷。
+ 用 v-for 更新已渲染过的元素列表时，它默认使用“就地复用”的策略。如果数据项的顺序发生了改变，Vue 不会移动 DOM 元素来匹配数据项的顺序，而是简单复用此处的每个元素。因此通过为每个列表项提供一个 key 值，来以便 Vue 跟踪元素的身份，从而高效的实现复用。这个时候 key 的作用是为了高效的更新渲染虚拟 DOM。
-------
#### 8.4.5.2   \[面试题] v-for与v-if为什么不推荐在同一元素上一起使用？
+ 当它们处于同一节点，v-for 的优先级比 v-if 更高，这意味着 v-if 将分别重复运行于每个 v-for 循环中。当你只想为部分项渲染节点时，这种优先级的机制会十分有用。
```html
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```
+ 如果此时todos列表的数据非常大，只要一个todo项发生改变，就会再次优先执行`v-for`循环遍历。**性能浪费比较大**。此时有更优的替代办法：
	- ① 使用一个计算属性`computed`, 返回一个过滤后的新的newTodos，这样过滤后的列表只会在newTodos 数组发生相关变化时才被重新运算，过滤更高效。使用`v-for="todo in newTodos` 之后，我们在渲染的时候只遍历新的项，渲染更高效。
	- ② 将 `v-if` 移动到外层容器元素。
> 详细内容参考:[《官方Vue:不推荐在同一元素上使用v-for和v-if》](https://cn.vuejs.org/v2/style-guide/#避免-v-if-和-v-for-用在一起必要)
------
### 8.4.6 🐯v-model指令

+ 在**表单**控件或者组件上创建双向绑定。
```html
<body>
  <div id='app'>
    <input type="text" v-model="nickname">{{nickname}}
  </div>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        nickname: 'Jamie'
      },
      methods: {}
    })
  </script>
</body>
```
+ 各种不同type的表单，具体绑定数据的实现参考官方文档:[表单输入绑定](https://cn.vuejs.org/v2/guide/forms.html#基础用法)。

  - 单选框 \<input type="radio">
    + 一组单选框控件，需要将v-model绑定给同一个变量，每个单选控件都要有一个`value`值。
    + 如果绑定的变量的值和某个单选项的`value`值相同，那么这个单选项被选中。
	- 一个复选框是否勾选 \<input type="checkbox">
	  + 绑定的变量是一个布尔值，无需设置`value`属性。
  - 多个复选框多选
    + 一组复选框控件，都要绑定给**同一个数组变量**。每个复选控件都要有一个`value`值。
    + 如果多选控件的`value`值存在于`v-model`绑定的数组中，那么该控件将会被选中。
  - 下拉列表\<select>\<option>\</select>  
    + v-model绑定给`select`标签，控件如果有`value`值就提取`value`值，如果没有`value`值就提取`option`标签内的文本。
    + 如果绑定的变量的值和某个单选项的`value`值相同，那么这个单选项被选中。
+ 输入框输入事件的修饰符
	- `.lazy`: 在默认情况下，v-model 在每次 input 事件触发后将输入框的值与数据进行同步 (除了上述输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步.
	- `.number`: 如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符。
	- `.trim`: 如果要自动过滤用户输入的首尾空白字符，可以给`v-model` 添加 `trim` 修饰符.
-------
#### 8.4.6.1 🚀 [面试题] 如何使用v-bind实现v-model指令创建的表单双向数据绑定？
```html
<body>
  <div id='app'>
    <input type="text" :value="nickname" v-on:input="change">{{nickname}}
  </div>
  <script>
    var vm = new Vue({
      el: '#app',
      data: {
        nickname: 'Sansa'
      },
      methods: {
        // 给input事件指定一个变更数据的函数
        change(e) {
          this.nickname = e.target.value;
        }
      }
    })
  </script>
</body>
```
------
#### 8.4.6.2 如何在自定义的组件上使用v-model双向数据绑定功能？

+ 自定义事件也可以用于创建支持 `v-model` 的自定义输入组件。

```js
// 注册一个自定义的组件
Vue.component('custom-input', {
  props: ['value'],
  template: `
    <input
      v-bind:value="value"
      v-on:input="$emit('input', $event.target.value)"
    >
  `
})
```
```html
<!-- 使用自定的组件 -->
<custom-input v-model="searchText"></custom-input>
```
> 更多资料参考：[《官方Vue:在组件上使用v-model》](https://cn.vuejs.org/v2/guide/components.html#在组件上使用-v-model)
------

### 8.4.7 🐯v-on指令

+ `v-on`指令用于**事件绑定**。
+ 缩写`@` 。
```html
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>
<!-- 缩写 -->
<button @click="doThis"></button>
```

+ 在监听**原生 DOM 事件**时，方法以事件为唯一的参数。如果使用内联语句，语句可以访问一个 `$event` property：`v-on:click="handle('ok', $event)"`。
```html
<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>
<script>
  new Vue({
    //...
    methods:{
      doThat(sth,e){
        // e 就是事件对象
      }
    }
  })
</script>
```
+ 用在**自定义元素组件**上时，也可以监听子组件触发的**自定义事件**。
```html
<!-- 子组件绑定事件指令~内联语句 -->
<my-component @my-event="handleThis(123, $event)"></my-component>
```
+ 事件修饰符
  + `.stop` ： 相当于原生JS的`stopPropagation`，停止冒泡。
  + `.prevent`:  相当于原生的`preventDefault`，阻止默认行为。
  + `.capture`：捕获阶段触发。
  + `.once`: 只触发一次。
  + `.self`: 只当事件是从侦听器绑定的元素本身触发时才触发回调。
  > 注意区别`按键修饰符`(对按键事件修饰)
```html
<!--  元素绑定事件指令~串联修饰符 -->
<button @click.stop.prevent="doThis"></button>
```

> 详细参考：[《官方Vue: 指令v-on》](https://cn.vuejs.org/v2/api/#v-on)

------

### 8.4.8 v-once指令

+ `v-once`指令：只渲染元素和组件**一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。
------
### 8.4.9 自定义指令
+ 有的情况下，你仍然需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令。
+ 注册指令的方法：全局注册和局部组件注册。
```js
// 全局注册
Vue.directive('指令名',{
  bind:(el,binding,vnode,oldnode){},
  inserted:(el,binding,vnode,oldnode){},
  update:(el,binding,vnode,oldnode){},
  componentUpdated:(el,binding,vnode,oldnode){},
  unbind:(el,binding,vnode,oldvnode){}
})
```

```js
// 局部注册
directives:{
  指令名:{
    bind:(el,binding,vnode,oldnode){},
    inserted:(el,binding,vnode,oldnode){},
    update:(el,binding,vnode,oldnode){},
    componentUpdated:(el,binding,vnode,oldnode){},
    unbind:(el,binding,vnode,oldvnode){}
  }
}
```
+ 指令钩子函数。比如`inserted`,被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
+ 指令钩子函数。比如`el`,被绑定的元素，`binding`，绑定信息对象。
> 更多资料参考:[《官方Vue:自定义指令》](https://cn.vuejs.org/v2/guide/custom-directive.html)
-------
## 8.5 Vue基础功能

### 8.5.1 🐯计算属性

+ 模板内的表达式非常便利，但是设计它们的初衷是用于简单运算的。在模板中放入太多的逻辑会让模板过重且难以维护，所以，对于复杂逻辑，应当使用**计算属性**`computed`。
```html
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 我们提供的函数将用作 property - vm.reversedMessage 的 getter 函数：
    reversedMessage: function () {
      // 翻转字符串
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```
+ 计算属性默认只有`getter`，不过在需要时(修改计算属性)也可以提供一个`setter`。

------
#### 8.5.1.1 🚀 [面试题]计算属性computed和methods有什么区别？
+ 我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是**计算属性是基于它们的响应式依赖（关联的变量值）进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。而methods没有缓存，每次访问都会重新执行。

------

### 8.5.2 🐯侦听器

+ Vue 通过 `watch` 选项提供了一个更通用的方法，来响应数据的变化。当需要在数据变化时执行异步或开销较大的操作时，这个方式是最有用的。

  ```js
  new Vue({
    //...,
    data:{
      sth:someValue
    },
    watch:{
      // 如果变量sth发生变化，就会执行函数
      sth:function(newValue,oldValue){
        //...
      }
    }
  })
  ```
+ 注意区分`浅监听`和`深监听`
	- 浅监听：监听的是基础数据类型，或简单纯数组。
	- 深监听：监听引用数据类型。
	```js
	// 深监听
  watch:{
    变量名:{
          deep:true,
          handler:function(newval){
           ...
          }
        }
	}
  ```
  > 也可以使用命令式的`vm.$watch` API。
------
#### 8.5.2.1 🚀[面试题]计算属性computed和watch的区别？
+ 共同点：都是以Vue的追踪依赖机制为基础的，都希望在依赖数据发生改变的时候，被依赖的数据根据预先定义好的函数，发生“自动”变化。
+ 不同点：
	- `watch`x 擅长处理的场景：一个数据影响多个数据；或需要在数据变化时执行**异步**或**开销较大**的操作时，使用watch侦听器最有用。
	- `computed`擅长处理的场景：一个数据受多个数据的影响，计算**逻辑比较复杂**，但操作开销较小，通常是为了简化模板里面的操作，对数据处理后进行展示。
------

### 8.5.3 过滤器

+ Vue.js 允许你自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：双花括号插值和 v-bind 表达式。过滤器应该被添加在 JavaScript 表达式的尾部，由“管道”符号`|`指示。`capitalize`过滤器函数将会收到 `message` 的值作为第一个参数。
```html
<!-- 在双花括号中 -->
<p>{{ message | capitalize }}</p>
<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | formatId"></div>
```
+ 可以在一个组件的选项中定义本地的过滤器`filters`,或者在创建 Vue 实例之前全局定义过滤器`Vue.filter`。

```js
// option【局部定义】
new Vue{
  data{
    //...
  },
  filters:{
    //
    capitalize: function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
}
```

```js
// 【全局定义】
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})
new Vue({
  //...
})
```
+ 过滤器可以串联使用
```html
{{ message | filterA | filterB }}
```

#### 8.5.3.1 计算属性computed和过滤器的区别

|    计算属性computed    | 过滤器filters |
| :--------------------: | :-----------: |
| 依赖于一个固定的实例，在某一个实例中使用 |    不依赖于实例。可以 定义一个全局过滤器，在多个实例中使用     |
| 不接受额外参数，依赖于data属性中的变量 | 不要求是data中的变量，可以是临时变量。可接受额外参数 |
| 有缓存管理机制，可减少页面调用次数 | 无缓存机制，调用次数，取决于页面中有所多少过滤器 |
| 计算属性虽默认为只读，但可以定义为对象，开启可读可写模式 | 只能读取操作 |
| 计算属性被作为一个类属性调用 | 过滤器被作为一个特殊方法处理 |

+ 共同点：都必须要有返回值。

------

### 8.5.4 动画

+ Vue 提供了 `transition` 的封装组件，在下列情形中，可以给任何元素和组件添加进入/离开过渡：
  - 条件渲染（v-if）
  - 条件展示（v-show）
  - 动态组件
  - 组件根节点
+ 动画状态：在进入和离开的过渡中，会有6个class切换。`transition`标签中的name属性会对应css中状态名的`name`。
	- 进入状态
	  + 进入前 name-enter
	  + 进入中 name-enter-active
	  + 进入后 name-enter-to
	- 离开状态
	  + 离开前 name-leave
	  + 离开中 name-leave-active
	  + 离开后 name-leave-to
+ 自定义过渡类名
  - 可以通过以下 attribute 来自定义过渡类名，他们的优先级高于普通的类名，这对于 Vue 的过渡系统和其他第三方 CSS 动画库，如 Animate.css 结合使用十分有用。
    + enter-class
    + enter-active-class
    + enter-to-class
    + leave-class
    + leave-active-class
    + leave-to-class
> 更多内容参考: [《官方Vue:动画》](https://cn.vuejs.org/v2/guide/transitions.html#单元素-组件的过渡)

------
## 8.6 Vue组件化开发

### 8.6.1 组件基础

#### 8.6.1.1 组件介绍

+ 组件是可复用的Vue实例。网页开发中的组件的含义，是具有独立功能的模块（HTML，CSS，JS）。
+ 网页中的一切结尾组件：大到一个页面，小到一个标签都是组件。
+ 组件化开发：
  - 组件可以复用，提高开发效率。
  - 维护容易。

------

#### 8.6.1.2 组件注册

+ 组件的注册，可以理解为创建一个`自定义标签`。
+ 全局注册`Vue.component('compName',{configObj})`。使用时，当做自定义的标签来使用。
```html
<head>
  <style>
    .itembox{
      border:2px solid #eee;
    }
  </style>
</head>
<body>
  <div id="app">
    <item></item>
    <item></item>
  </div>
</body>
<script>
// 全局注册
Vue.component('item',{
  template:"<div class='itembox'><h1 class='tit'>标题</h1><div class='cont'>内容</div></div>"
})
</script>
```
+ 局部注册
  ```js
  new Vue({
    //...,
    components:{
      compName1:{
        configOpts
      },
      compName2:{
        configOpts
      }
    }
  })
  ```
```js
new Vue({
  el:'#app',
  data:{},
  components:{
    warn:{
      template:"<div class='warning'>警告：xxxx</div>"
    }
  }
})
```
+ 注册组件命名时，既可以使用短横线（kebab）命名法，又可以使用驼峰命名法。但是在直接在 DOM (即非字符串的模板) 中使用时只有 kebab-case 是有效的。
+ 组件模板`template`的值，可以使用模板字符串，也可以用选择器去匹配对应的template模板内容。
+ Vue2组件模板只能有一个根标签。即，\<template>标签内层的根节点只能有一个。
------

#### 8.6.1.3 组件嵌套

+ **组件嵌套**是指，在一个组件的`template`模板里面使用另一个组件。注意和"插槽”区分。
```js
Vue.component('ccc',{
  template:`<div>我是ccc <ddd></ddd> </div>`,
  components:{
    ddd:{
      template:`<div>我是ddd <eee></eee> </div>`,
      components:{
        ee:{
          template:`<div>我是eee</div>`
        }
      }
    }
  }
})
```
+ 全局组件，任何组件的模板里面都可以使用。局部组件，只能出现在自己的父模板里面！
------

#### 8.6.1.4 组件的配置选项

+ template 类似 el 
+ components
+ data
  - vue实例中，data是一个对象，而**组件实例data是一个函数！且这个函数返回一个对象**。
+ methods
+ watch
+ filters
+ directives
+ ...
------
##### 8.6.1.4.1 🚀[面试题]为什么组件实例的data必须是一个函数？

> 注意区分new Vue() 生成的vue实例对象和组件实例的区别。
+ 组件复用时，所有组件实例都会共享data，如果data是对象的话，就会造成一个组件修改data以后会影响到其他所有组件，所以必须把data写成一个函数，返回的每个实例可以维护一份被返回对象的独立的拷贝。
------
#### 8.6.1.5 动态组件

+ 有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里切换tab。动态切换组件可以通过 Vue 的 `<component>` 元素加一个特殊的 `is` attribute 来实现。
```html
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component :is="currentTabComponent"></component>
```
+ 动态组件切换时结合动画。
```html
<!--引入animate.css 库 -->
<transition enter-active-class="animated fadeIn" leave-active-class="animated fadeOut">
  <component :is="curTabComp"> </component>
</transition>
```
+ `is`属性在标签上静态使用时，可以用来替换当前的默认标签。
```html
<div id="app">
  <ul>
    <!-- li会被替换 -->
    <li is="libox"></li>
  </ul>
</div>
<script>
 new Vue({
   el:"#app",
   data:{
   //...
   },
   components:{
     libox:{
       template:`<li>
                  <h3>我是libox组件<h3>
               </li>
       `
     }
   }
 })
</script>
```

------
#### 8.6.1.6 缓存组件

+ 使用\<keep-alive>标签包裹动态组件，那么组件实例能够被在它们第一次被创建的时候缓存下来。当频繁切换组件的时候，可以避免反复重渲染导致的性能问题。使用了缓存组件后，`created`只会执行一次。
```html
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```
+ 注意两个生命周期函数：`activated`和`deactivated`，针对特定的不想使用缓存的组件，可以使用这两个钩子函数来执行一些操作。
	- `activated`: 被`keep-alive`缓存的组件**激活**时调用。
	- `deactivated`: 被`keep-alive`缓存的组件**停用**时调用。
> 注意上面两个钩子函数在服务器端渲染不会调用。
-------

### 8.6.2 脚手架

+ `Vue-Cli` 即Vue.js开发的标准化工具。

+ 安装
```bash
sudo npm i -g @vue/cli
```
+ 创建项目
```bash
vue create my-project
```
------
#### 8.6.2.1 [面试题] vue-cli工程都用到了那些技术？
+ vue.js : vue-cli 的核心。主要特点是双向数据绑定和组件系统。

+ vue-router: Vue官方推荐使用的路由框架。

+ vuex： 专门为Vue.js项目开发的状态管理器，主要用于维护vue组件公用的一些变量和方法。

+ webpack： 模块加载和vue-cli工程打包器。

+ 创建一个`emit.js`，用于vue事件机制的管理。
------
#### 8.6.2.2 vue-cli 4创建的工程项目文件关系梳理

<div align="center">
  <img src="./0_pictures/Vue-cli4_relationMap.png" alt="relationMap" />
</div>
-------

### 8.6.3 组件通信

#### 8.6.3.1 🐯父传子： props

+ 父组件通过**属性传值**给子组件；子组件内部通过**`props`**这个option来**接收**传递过来的值。

  ```html
  <!-- 父组件 -->
  <menu-item :title="titleVal"></menu-item>
  <menu-item menu-title="titleVal"></menu-item>
  ```

  ```js
  <!-- 子组件:【全局注册】的形式 -->
  Vue.component('menu-item',{
    props:['title','menuTitle'],
    template:`<div> {{title}}--{{menuTitle}} </div>`
  })
  ```
  ```vue
  <!-- 子组件:复杂项目开发中的【单文件组件】形式 -->
  <template>
  	<div> {{title}}--{{menuTitle}} </div>
  </template>
  <script>
  export default {
    name:'menu-item',
    props:['title','menuTitle'],
    data(){
      return{}
    }
  }
  </script>
  ```

  - 如果希望每个prop都有指定的值的类型，或其他限制时，可以以对象的形式列出prop。因为数组形式的声明props只能声明属性是否存在，而不能附加其他限制。

    ```js
    export default {
      name:'menu-item',
      props:{
        title:String,// 单类型
        menuTitle:[String,Number],// 多类型
        propA:{
          type:String,
          default:'默认值', // 未传值时，使用默认值
        },
        propB:{
          type:Number,
          required:true // 必填
        },
        propC:{
          type:Array,
          //default 如果自定义属性是基本数据类型，那么直接写值；如果是一个【引用数据类型】，需要写成一个函数，且这个函数返回一个数组或对象。
          default(){
            return [];
          }
        },
        propD:{
          type:Array,
          // 自定义校验函数！函数要返回一个布尔值，表示验证成功与否
          validator(val){ 
            return val.length>2?true:false
          }
        }
      },
      data(){
        return{}
      }
    }
    ```

  - 所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**的数据流：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会**防止从子组件意外变更父组件的状态，从而导致你的应用的数据流向难以理解**。

  - JS中的`props`使用驼峰命名法（camelCase）对应DOM模板中使用短横线命名法（kebab-case）。

------

#### 8.6.3.2 🐯子传父： 自定义事件 

+ 子组件通过`自定义事件$emit()`向父组件传递信息；父组件中监听**子组件自定义事件名**的事件。

+ 子组件使用 $emit 的第二个参数来提供一个传给自定义事件的参数，当在父级组件监听这个事件的时候，我们可以通过` $event` 访问到被抛出的这个值。
  ```html
  <!--子组件 触发一个名为'enlarge-text'的自定义事件 -->
  <button @click="$emit('enlarge-text',0.1)">增大字体</button>
  ```
  ```html
  <!--父组件中，监听名为'enlarge-text'的事件 -->
  <menu-item v-on:enlarge-text="fontSize +=$event"></menu-item>
  ```
  
+ 注意，自定义事件名应当始终使用短横线命名` kebab-case` 的事件名。

------

#### 8.6.3.3 祖传子

+ **`provide`**和**`inject`**两个选项option：一个祖先组件向其所有子孙后代组件注入一个依赖，不论组件层次有多深，并在其上下游关系成立的时间里始终生效。
+ `provide` 选项应该是一个对象或返回一个对象的函数。该对象包含可注入其子孙的 property。`inject` 选项应该是一个字符串数组或一个对象。
```js
// 祖先组件
export default {
  data(){
    return {}
  },
  provide:{
    msg:'祖先',
    age:88
  }
}
```

```js
// 子组件
export default {
  //...
  // inject:['msg','age']
  inject:{
    bar:{
      from:'aaa',
      default:'默认值'
    // default:()=>[1,2,3]
    }
  }
}
```
------
#### 8.6.3.4 非父子组件通信
+ 实现任意两个组件的通信。核心：利用了*vue实例的原型对象*里面的$\$emit$和$\$on$实现组件的监听事件和触发事件。
```js
// main.js
let bus = new Vue();
// 在Vue的原型上面添加数据，所有的组件都可以获取到
// 所有的组件实例都将会共享一个$bus的对象
Vue.prototype.$bus=bus;
```
```js
// 任意组件A: 监听事件
create(){
  this.$bus.$on('eventName',(args)=>{})
}
```
```js
// 任意组件B: 触发事件
this.$bus.$emit('eventName',args)
```
------
#### 8.6.3.5 父子关系
+ \$parent , \$children

+ [$ref ](#ref)： 获取到当前组件中的子组件或元素作为dom节点，直接引用这个节点然后灌入数据。
  - 注意：**mounted()**钩子函数之后再调用，因为这个时候才存在真实DOM节点。

------

### 8.6.4 组件插槽

+ 插槽的含义：在组件的标签之间放内容，该组件内部的模板中可以存在`<slot>`标签，标签之间的内容会替换slot标签。

  > v-slot 指令自Vue2.6.0版本后引入，提供了更好的slot和slot-scope attributed的API替代方案。

#### 8.6.4.1 组件插槽的基本用法

+ 如果\<slot>标签内本来就存在content，那么这里的content将会被作为后备内容。
+ 父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。

```js
// 子组件定义插槽slot的位置
Vue.component('alert-box',{
  template:`
    <div class="demo-alert-box"> 
      <strong>Error!</stong>
      <slot></slot>
    </div>
  `
})
```
```vue
<!-- 父组件中使用子组件 -->
<alert-box>Something bad happened.</alert-box>
```
--------
#### 8.6.4.2 具名插槽
+ 对于多个插槽匹配，可以使用具名插槽的语法。在向具名插槽提供”定制内容“的时候，可以在\<template>标签上使用`v-slot`指令，并以`v-slot:slotName`的形式匹配子组件内部的插槽名称。`v-slot`指令只能使用在\<template>标签上。
+ 在子组件内部的\<slot>标签上，可以提供`name="slotName"`的属性，用于定义插槽名称。
+ 一个不带 name 的 <slot> 出口会带有隐含的名字“default”。任何没有被包裹在带有 `v-slot` 的 <template> 中的内容都会被视为默认插槽的内容。
```vue
<!-- 子组件内部:定义插槽的位置和名称 -->
<template>
  <div class="container">
    <header>
      <slot name="header"></slot>
    </header>
    <main>
      <slot></slot>
    </main>
    <footer>
      <slot name="footer"></slot>
    </footer>
  </div>
</template>
<script>
export default {
  name:'base-layout',
  data(){return{}}
  //...
}
</script>
```
```vue
<!-- 父组件中使用子组件 -->
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <p>A paragraph for the main content.</p>
  <p>And another one.</p>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```
-------
#### 8.6.4.3 作用域插槽

+ 作用域插槽实现了这样的功能：<span style="color:red">让父级中的插槽内容能够访问子组件中才有的数据</span>。默认情况下，父级内容无法访问子组件中的数据。
	- 在子组件中的\<slot>标签上,使用v-bind绑定想要和父级分享的数据对象。这个绑定在 <slot> 元素上的 attribute 被称为**插槽 prop**。
	- 在父级的\<template>标签上, 我们可以使用带值的 v-slot 来定义子组件提供的数据对象的名字，比如`v-slot:default="slotProps"`。
> "slotProps"的名称可以任取，代表从子组件插槽传递过来的数据，也可以说“slotProps”代表着从子组件中劫持过来的作用域，子组件中指定的数据都可以通过`slotProps.X`的形式在父组件的环境中访问。
```vue
<!-- 子组件 -->
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```
```vue
<!-- 父组件中使用子组件 -->
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```
+ 当被提供的内容只有默认插槽时，组件的标签才可以被当作插槽的模板来使用。`v-slot:default="slotProps"`可以缩写为`slot="slotProps"`。

```vue
<current-user v-slot:default="slotProps">
  {{ slotProps.user.firstName }}
</current-user>
```
+ 只要出现多个插槽，请始终为所有的插槽使用完整的基于 <template> 的语法。
```vue
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>

  <template v-slot:other="otherSlotProps">
    ...
  </template>
</current-user>
```
--------
## 8.7 Vue插件

+ 插件通常用来为 Vue 添加**全局**功能。

### 8.7.1 Vue路由:VueRouter

+ VueRouter是一个实现了路由管理的官方库。
> vue-router是Vue.js官方的路由插件，它和vue.js是深度集成的，适合用于构建单页面应用。vue的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。传统的页面应用，是用一些超链接来实现页面切换和跳转的。在vue-router单页面应用中，则是路径之间的切换，也就是组件的切换。**路由模块的本质 就是建立起url和组件之间的映射关系**。至于我们为啥不能用a标签，这是因为用Vue做的都是单页应用（当你的项目npm run build 打包后，就会生成dist文件夹，这里面只有静态资源和一个index.html页面），所以你写的\<a>\</a>标签跳转页面是不起作用的，你必须使用vue-router来进行管理。

+ 🪴关于Vue路由，重点掌握两个标签**\<router-view>**和**\<router-link>** 以及两个对象**this.$route**和**this.$router**。
#### 8.7.1.0 基础配置参考
+ 安装
```bash
# 注意，vue脚手架开发，初始项目的时候就要配置安装好vue-router
npm i vue-router --save
```
+ 引入后使用：src >> router >> index.js
```js
import Vue from "vue"
import VueRouter from "vue-router"
Vue.use(VueRouter)

// 导入页面组件
import index from "@/pages/index"
import car from "@/pages/car"
import user from "@/pages/user"

// 实例化一个路由对象
let router = new VueRouter({
  routes:[ // 路由映射数组
    { path:"/",component:index },
    { path:"/car",component:car },
    { path:"/user",component:user }
  ]
})

export default router
```

+ 在main.js中注入路由信息
```js
import Vue from 'vue';
import App from './App';

import router from './router';
new Vue({
  el:'#app',
  router,
  components:{App},
  template:'<App/>'
})
```
+ 在App.vue模板里面写入`router-view`标签，路由地址切换后对应的组件内容就展示在这里。
+ 使用`router-link`实现a标签。to的值就是 *路由映射配置里的path的值*。

------
#### 8.7.1.1 404配置
+ 默认情况下，访问一个没有被定义的路由地址，页面不会渲染任何内容。我们可以给它指定到404页面。
+ 404页面的配置放在最后。
+ 实现：
```js
{
 routes:[
   {
     path:'/',         // 路由地址
     name:'index',     // 路由名称
     component:Index   // 路由组件
   },
   //...
   {
     // * 表示匹配任意路径，放在routes匹配规则的最后，通常用于匹配客户端 404 错误页面
     path:'/*',
     component:404组件
   }
 ]
}
```
------
#### 8.7.1.2 命名路由
+ 创建 Router 实例的时候，在 routes 配置中给某个路由设置名称`name`。
```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
+ 如何跳转到`命名路由`？
  - 给 `\<router-link>` 的 `to` 属性传一个对象
  ```vue
  <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
  ```
  - 同js代码调用 `router.push()`
  ```js
  router.push({ name: 'user', params: { userId: 123 } })
  ```
------
#### 8.7.1.3  \<router-link> 
+ \<router-link> 组件支持用户在具有路由功能的应用中 (点击) 导航。 通过 `to` 属性指定目标地址，默认渲染成带有正确链接的 \<a> 标签，可以通过配置 `tag` 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。

+ `to属性`: to属性表示目标路由的链接。当被点击后，内部会立刻把 `to` 的值传到 `router.push()`，所以这个值可以是一个字符串或者是描述目标位置的对象。

  ```html
  <!-- 字符串 -->
  <router-link to="home">Home</router-link>
  <!-- 描述目标位置的对象 -->
  <router-link :to="{ path: 'home' }">Home</router-link>
  <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
  ```

+ `replace`属性: 设置 replace 属性的话，当点击时，会调用 router.replace() 而不是 router.push()，于是导航后不会留下 history 记录。

  ```vue
  <router-link :to="{ path: '/abc'}" replace></router-link>
  ```

+ 激活的类名 : 设置链接激活时使用的 CSS 类名.
  - 默认值: "router-link-active",表示**模糊匹配**。这个默认值可以通过路由的构造选项 **`linkActiveClass`** 来全局配置。
  ```vue
  <router-link to="/" tag="li">首页</router-link>
  <router-link to="/user">用户</router-link>
  ```
  ```html
  <!--上述两个标签渲染后都会加上router-link-active类名 -->
  <li class="router-link-active">首页</li>
  <a class="router-link-active">用户</a>
  ```
  - 默认值: ”router-link-exact-active"表示**精确匹配**。这个默认值可以通过路由的构造选项 **`linkExactActiveClass`** 来全局配置。”精确匹配“可以在\<router-link>标签上添加`exact`属性来开启。
  ```vue
  <router-link to="/news/info" exact>新闻详情</router-link>
  ```
  ```html
  <!-- 仅在连接到"/news/info"时激活”router-link-exact-active"类名-->
  <a class="router-link-exact-active">新闻详情</a>
  ```
  - 全局配置(更改)激活的类名：
  ```js
  export default new Router({
    linkActiveClass:"自定义class名1", // 模糊匹配路由成功后，激活的class名
    linkExactActiveClass:"自定义class名2",// 精确匹配路由成功后，激活的class名
    routes:[
      { path:'/a',component:'aComp'},
      { path:'/b',component:'bComp'},
    ]
  })
  ```
------
#### 8.7.1.4 重定向和别名
+ `重定向`也是通过 `routes` 配置来完成.下面例子是从 /a 重定向到 /b
```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})
```
+ `别名`:/a 的别名是 /b，意味着，当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。
```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```
------
#### 8.7.1.5 动态路由
+ 使用场景：新闻列表到新闻详情，详情需要知道新闻id；
+ 动态路由：使用一个以冒号开头的动态路径参数来匹配同一个组件，这个组件用不同的数据来渲染。当匹配到一个路由时，参数值会被设置到 `this.$route.params`，可以在每个组件内使用。
```js
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```
> 详情参考：[Vue官方：动态路由匹配](https://router.vuejs.org/zh/guide/essentials/dynamic-matching.html#响应路由参数的变化)
------
#### 8.7.1.6 区分两个重要对象

> 在main.js 中将路由挂载到vue实例上面后，所有的组件对象里面都会有两个属性，$route和\$router。

+ `$route` : 表示当前页面的路由状态信息。
  - $route.path: 当前路由的绝对路径。
  - $route.params:一个 key/value 对象, 包含了动态片段和全匹配片段。
  - $route.query: 一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象。
  - $route.hash: 当前路由的 hash 值 (带 `#`) ，如果没有 hash 值，则为空字符串。
  - $route.name: 当前路由的名称。
  - $route.meta: 当前路由的[元信息](#metaFields)。
> 详情参考：[官方Vue:路由对象](https://router.vuejs.org/zh/api/#路由对象)
+ `$router`: 表示整个全局的路由实例对象。它是VueRouter构造函数的实例对象。
> 详情参考：[官方Vue:Router实例](https://router.vuejs.org/zh/api/#Router实例属性)
------

#### 8.7.1.7 编程式导航
+ 借助`this.$router`对象上的方法，可以导航到不同的URL。
+ ① `$router.push()`:这个方法会向 `history` 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。
  - 该方法的参数可以是一个字符串路径，或者一个描述地址的对象。
  - 注意：如果提供了 `path`，那么`params` 会被忽略。
+ ② `$router.replace()`:跟 `router.push` 很像，唯一的不同就是，它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 `history` 记录。
+ ③ `$router.go(n)`: 这个方法的参数是一个整数，意思是在 `history` 记录中向前或者后退多少步，类似 ·window.history.go(n)`。
> 详情参考：[官方Vue:编程式导航](https://router.vuejs.org/zh/guide/essentials/navigation.html)
------
#### 8.7.1.8 命名视图

+ 这里的"视图"指的就是\<router-view>。

+ 默认情况下，一个视图使用一个组件渲染，因此对于同个路由，**多个视图就需要多个组件**。通过给\<router-view>命名，添加name属性，并完善**`components`**对象里的的映射关系，可以实现命名视图。
```vue
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
```
```js
const router = new VueRouter({
  routes: [
    {
      path: '/new',
      // ! 注意是 components  
      components: {
        default: New,
        a: Bar,
        b: Baz
      }
      // 不仅渲染了New组件，同时也在匹配的位置渲染了Bar和Baz组件
    }
  ]
})
```
------
#### 8.7.1.9 嵌套路由
+ URL 中各段动态路径也可以按照按某种结构对应嵌套的各层组件。
+ 顶级组件中的`<router-view>` 是最顶层的出口，渲染最高级路由匹配到的组件。同样地，一个被渲染组件同样可以包含自己的嵌套 `<router-view>`。
+ 要在嵌套的出口中渲染组件，需要在 `VueRouter` 的参数中使用 **`children`** 配置。同时，该层级的路由组件中必须要有\<router-view>标签来对应子组件的渲染位置。
+ 以 `/ `开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径（不以`/` 开头表示可以继承上级的路径）。
+ 嵌套路由无法直接通过表示多层级的path字符串("a/b/c")，直接写在顶级routes下。顶级routes下的并列属性只能对应顶级组件的\<router-view>。
+ 当前层级可以通过配置`redirect`属性来配置多层级的路径字符串，以表示当前层级默认要渲染的子层级。
```js
const router = new VueRouter({
  routes:[
    //...
    {
      path:'/user',
      component:User,
      name:"user",
      // 指定当前层级默认渲染出的下一层级组件
      redirect:'/user/order',
      children:[
        { path:'/user/order',component:User},
        { path:'/user/class',component:Class},
        { path:'/user/set',component:Set}
      ]
    }
  ]
})
```
+ 也可以在`children`中,指定path为`/`或空字符串，指定一个单独的组件`component`来匹配父级路由的默认渲染子级视图。
```js
const router = new VueRouter({
  routes:[
    //...
    {
      path:'/user',
      component:User,
      name:"user",
      children:[
        // 指定：父级path对应的默认渲染子级视图。它会替换父级组件的<router-view>位置
        { path:'/',component:Individual},
        { path:'/user/order',component:User},
        { path:'/user/class',component:Class},
        { path:'/user/set',component:Set}
      ]
    }
  ]
})
```

> 详细参考: [官方Vue:嵌套路由](https://router.vuejs.org/zh/guide/essentials/nested-routes.html)
------
#### 8.7.1.10 路由元信息
+ <span id="metaFields">在定义路由的时候可以单独配置`meta`字段。</span>
```js
const router = new VueRouter({
  {
    path:'/somewhere',
    component:'someComp',
    // 元信息：表示给这个路由设置一些自定义属性
    meta:{
      requrieAuth:true
    }
  }
})
```
+ 我们称呼`routes` 配置中的每个路由对象为*路由记录*。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录。一个路由匹配到的所有路由记录会暴露为 $route 对象 (还有在导航守卫中的路由对象) 的 `$route.matched` 数组。因此，我们需要遍历 `$route.matched` 来检查路由记录中的 meta 字段。
------
#### 8.7.1.11 导航守卫
+ vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。有多种机会植入路由导航过程中：全局的, 单个路由独享的, 或者组件级的。

+ 【全局前置守卫】 🐯**router.beforeEach**
  - 路由进入之前会执行的函数。通常会在这里做页面标题，或权限判断。
  ```js
  router.beforeEach((to,from,next)=>{
    // to: 即将要进入的路由目标对象
    //from: 当前正要离开的路由
    // next : function，一定要调用这个方法来resolve这个钩子(进行管道中的下一个钩子).
    next();
  })
  ```
  - 也可以在此处控制滚动行为
  ```js
  router.beforeEach((to,from,next)=>{
    // 从b页某纵深处跳转到a页顶部
    window.scrollTo(0,0);
    next();
  })
  ```
+ 【全局后置守卫】 router.afterEach
  - 路由离开后要执行的函数。
  ```js
  router.afterEach((to,from)=>{
    // to: 即将要进入的路由目标对象
    //from: 当前正要离开的路由
  })
  
  ```
+ 【路由独享的守卫】 beforeEnter
  - 在“路由记录”上配置。
  ```js
  const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
   ]
  })
  ```
+ 【组件内守卫】 beforeRouteEnter
+ 【组件内守卫】 beforeRouteUpdate
+ 【组件内守卫】 beforeRouteLeave
	- 以上三种组件内守卫在组件内定义
	```js
	const Foo = {
  template: `...`,
  beforeRouteEnter(to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`,也不能操作data里面的数据
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate(to, from, next) {
    // 在当前路由改变，但是该组件【被复用】时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave(to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
	```

> 详情参考：[Vue官方:导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#全局前置守卫)
-------
##### 8.7.1.11.1 vue-router同路由跳转同路由有什么问题？
+ vue-router同路由跳转同路由，组件将不再更新，不会再被创建一次。我们一般把请求数据放在created里面去执行，但是详情页面跳转到详情页面，组件不会被再次创建，created钩子里面的请求数据函数不再执行。
+ 解决方法：
	- ① beforeRouteUpdate
	- ② 侦听器watch监听$route的变化👌
	```js
	watch:{
	  $route:{
	    handler:'getData',
	    deep:true,
	    immediate:true  // 第一次也执行
	  }
	}
	```
-------
#### 8.7.1.12 路由懒加载
+ 原来的配置文件里面都是通过import导入所有的组件，这样做，应用开始的时候就会导入这些所有的组件。增加了负担。
+ 如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了，这就是**路由懒加载**技术。
```js
// 默认配置的路由
import compName from "@pages/compName";
var router = new VueRouter({
  routes:[
    {
     path:'/somewhere',
     component:compName
    }
  ]
})
```
```js
// 【路由懒加载】
var router = new VueRouter({
  routes:[
    {
     path:'/somewhere',
     component:()=>import("@/pages/Index")
    },
    {
     path:'/somewhere2'
     component:{
       default:()=>import("@/pages/New"),
       newsInfo:()=>import("@/pages/Info")
     }
    }
  ]
})
```
```js
// 结合webpack，定义一个能够被 Webpack 自动代码分割的异步组件。
const Foo = () => import('./Foo.vue');
const router = new VueRouter({
  routes: [{ path: '/foo', component: Foo }]
});
```

------
#### [面试题] 🚀 Ⅰ. 如何实现两个页面之间传参？
+ 动态路由。动态路径参数的值都会设置到`this.$route.params`中。

  ```vue
  <!--页面a中声明一个跳转链接-->
  <router-link to="/somewhere:id"></router-link>
  ```

  ```js
  // 跳转到b页面后，可以访问到【params】参数
  this.$route.params.id
  ```

+ query传参。在`this.$route.query`对象中保存着query数据。
  ```vue
  <!--页面a中申明一个跳转链接-->
  <router-link to="/somewhere?key=val"></router-link>
  ```
  ```js
  // 跳转到b页面后，可以访问到【query】参数
  this.$route.query.key
  ```
  
+ 本地存储。
  ```js
  // 页面a中设置一个数据
  localStorage.setItem('key',val)
  ```
  ```js
  // 页面b中拿到数据
  localStorage.getItem('key')
  ```
------
#### [面试题] 🚀 Ⅱ . 命名视图\<router-view>和动态组件\<components :is>都能实现组件的切换显示，它们有什么区别？
+ \<router-view> 是vueRouter的组件，会更新路由，可以不用跳转到一个新的页面，不会更新初始化函数mounted和created，只更新\<router-view>\</router-view>标签下所渲染的组件。可以不用引入组件。
+ \<component :is> 是vue的组件，is=要渲染的组件，不会更新路由，不会更新初始化函数mounted和created。刷新页面后会跳回默认显示页面；需要引入所有要显示的组件，并且components:{声明}。
+ 两者的本质没什么不同，都是动态渲染组件，只是路由封装了url与展示的组件的关系，并可以切换任意多级组件，用动态组件做很难实现。
> 路由的改变是根据URL的状态改变而改变，所以要改变路由视图必须要对应URL的改变。并且路由的页面必须要现在routes里注册;而动态组件\<component :is="componentA" :prop="prop"> 仅仅是绑定了变化的组件，当然这些组件也必须在它们的父组件里注册，视图改变URL不需要发生改变。并且相对路由页面更加灵活。
-------
#### [面试题] 🚀 Ⅲ . 实现页面跳转有哪些方式？
+ 原生js:
  - a标签的href属性赋值url;
  - window.location赋值url;
  - location对象的href属性，或location的assign()方法。
  - history.back()  history.go() history.forward()。
+ vue:
  - $router.push();
  - $router.replace();
  - $router.go;
  - $router.back;
  - $router.forward;
-------
### 8.7.2 Vue状态管理:Vuex

+ Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。

+ 安装Vuex
  ```bash
  npm i vuex --save
  ```
+ 使用Vuex创建一个store实例。
```js
// src/store/index.js
// 实例化一个仓库对象
import Vue from "vue";
import Vuex from "vuex";

Vue.use(vuex);
let store = new Vuex.Store({
  state:{// 这里存放所有组件共享的数据，类似于所有组件的公用data
    msg:'Hello world'
  },
  getters:{},
  mutations:{},
  actions:{},
  modules:{}
});

export default store;
```
+ 给Vue 实例提供创建好的 store：以“store”选项的方式注入。
```js
// main.js
import Vue from 'vue';
import App from './App';
import router from './router';
import store from "./store";
//Vuex 提供了一个从根组件向所有子组件，以 store 选项的方式“注入”该 store 的机制：
new Vue({ 
  el: '#app',
  store,
  router,
  components:{App},
  template:'<App/>'
});
// 这样在所有的Vue组件中都可以通过访问this.$store
```
------
<div align="center">
  <img src="./0_pictures/vuex.png" alt="vuex" />
</div>
------
#### 8.7.2.1 State
+ `state`表示状态对象，这一个对象就包含了全部的应用层级状态。这里存放着所有组件共享的数据，类似于所有组件的共享data。
+ 由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在**计算属性中返回**某个状态。
```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```
+ **`mapState()`辅助函数**：当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性。相当于把store中的state里面的属性映射到当前组件的计算属性中。当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组。
```vue
<template>
 <div id="app">
   <p>{{msg}}=={{str}}</p>
 </div>
</template>
<script>
import { mapState } from "vuex";
export default {
  //...
  computed:{
    ...mapState(['msg','str'])
  }
}
</script>
```
------
#### 8.7.2.2 Getters
+ `getters`相当于一个存放全局计算属性的对象。这里可以对`state`中的数据状态进行派生。
```js
const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})
```
+ 我们可以在组件中通过`this.$store.getters`对象访问到getters。注意，getter 在通过属性访问时是作为 Vue 的响应式系统的一部分缓存其中的；getter在通过方法访问时每次都会去进行调用，而不会缓存结果。

+ `mapGetters`辅助函数，辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。
```js
// 组件中
import { mapGetters } from 'vuex'
export default {
  // ...
  computed: {
    ...mapGetters(['doneTodosCount','anotherGetter'])
  }
}
```
------
#### 8.7.2.3 Mutations

+ 修改`state`里面的数据的**唯一方法**是提交`mutation`.通过定义在`mutations`里面的方法修改state里的数据。
> 切记不要在组件中赋值state来修改数据。
```js
const store = new Vuex.Store({
  state:{
    msg:'Hello JS'
  },
  mutations:{
    // state 作为第一个参数，第二个参数是调用该方法传入的参数
    changeMsg(state,data){
      state.msg=data;
    }
  }
})
```
+ 在组件中，通过**`store.commit()`**提交`mutation`。第一个参数是mutation方法名，第二个参数是载荷对象，
```js
export default {
    //...
  methods:{
    changeMSG(){
      this.$store.commit('changeMsg','Hello Vue!')
    }
  }
}
```
+ 利用辅助函数**`mapMutations`**。将定义在store的`mutations`中的方法映射到当前组件的`methods`中，那么当前的组件就可以直接使用这些方法了。

```vue
<template>
 <div id="app">
   <p>{{msg}}----{{str}}</p>
   <button @click="changeMsg('HelloMsg')">点击修改</button>
   <button @click="changeStr('HelloStr')">修改str</button>
 </div>
</template>
<script>
import { mapMutations } from "vuex";
export default {
  //...
  // 【映射mutations】
  methods:{
    ...mapMutations(['changeMsg','changeStr'])
  }
}
</script>
```
+ 注意，`mutations`里面的函数一定执行的是同步操作！
------
#### 8.7.2.4 Actions
+ `action`类似`mutation`，也可以用来修改`state`中的数据。但是，`action`通过提交`mutation`来间接修改`state` ; `action`可以包含任意**异步**的操作，里面通常存放大量的异步逻辑请求代码。
```js
const store = new Vuex.Store({
  state: {
    count: 0,
    msg:''
  },
  mutations: {
    increment (state) {
      state.count++
    },
    changeSth(state,data){
      state.msg= data
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    },
    changeSth(ctx,data){
      ctx.commit('changeSth',data)
    }
  }
})
```
+ `Actions`中的函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 **`context.commit` **提交一个 `mutation`，或者通过 `context.state` 和 `context.getters` 来获取 `state` 和 `getters`。实践中，我们会经常用到 ES2015 的`参数解构`来简化代码。
```js
actions: {
  increment ({ commit },data) {
    commit('increment',data)
  }
}
```
+ 使用**`store.dispatch`**来触发`action`。
```js
// 触发action
store.dispatch('incrementAsync', {
  amount: 10
})
```
+ 可以使用`mapActions`辅助函数，将store中的actions映射到当前组件的方法`methods`中，那么可以在当前组件实例中直接使用该方法。
```js
// 组件中【映射actions】
import { mapActions } from 'vuex';
export default {
  // ...
  methods: {
    ...mapActions(['increment', 'incrementBy']),
    // 将actions中的'increment’方法映射为当前组件的add方法
    ...mapActions({ add: 'increment'})
  }
}
```
-----
#### 8.7.2.5 Modules
+ 针对大型项目，放在一个根模块，状态的维护困难。Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割。

+ 基本结构
```js
// store/index.js
let store = new Vuex.Store({
  state:{ 根state },
  getters: {根getters },
  mutations: { 根mutations },
  actions: {根actions },
  modules:{
    moduleA:{
      namespaced:false/true, // 不写默认就是false
      state:()=>({模块state}),
      getters: { 模块getters },
      mutations: { 模块mutations },
      actions: { 模块actions }
    },
    moduleB:{
      namespaced:false/true, 
      state:()=>({模块state}),// 注意模块的state是一个函数，这个函数返回一个对象
      getters: { 模块getters },
      mutations: { 模块mutations },
      actions: { 模块actions }
    }
  }
})
```
+ 真实结构
  - store 【目录】
    + index.js 仓库对象文件
    
      ```js
      // 导入根配置
      import state from './state';
      import getters from './getters';
      import mutations from './mutations';
      import actions from './actions';
      // 导入模块
      import moduleA from './moduleA';
      import moduleB from './moduleB';
      let store = new Vuex.Store({
        state,
        getters,
        mutations,
        actions,
        modules:{
          moduleA:moduleA,
          moduleB:moduleB
        }
      })
      ```
    
    + state.js  根state
    
      ```js
      export default {
        key:val
      }
      ```
    
    + getters.js  根getters
    
      ```js
      export default {
        newkey(state){ //根state
          return state.key
        },
        newkey1(state){
          return state.moduleA.key
        },
        newkey2(state){
          return state.moduleB.key
        }
      }
      ```
    
    + mutations.js  根mutations
    
      ```js
      export default {
        FN(state,data){
          state.key=data;
        }
      }
      ```
    
    + actions.js  根actions
    
      ```js
      export default {
        fn(store,info){
          store.commit('FN',info)
        }
      }
      ```
    
    + modules 【目录】
      - moduleA.js
      
        ```js
        export default {
          state:()=>({ 模块state } ),
          getters: { 模块getters },
          mutations: { 模块mutations },
          actions: { 模块actions }
        }
        ```
      - moduleB.js

+ 获取数据
```js
// 获取state
{{$store.state.key}} // 取根state里面的数据
{{$store.state.moduleA.key}} // 取模块A里面的数据
// 获取getters
{{$store.getters.key}} // 根getters里的数据
{{$store.getters.key2}} // 根getter的数据
// 所有的getters都会被挂载到根getters上面
```
+ 命名空间：**namespaced:false**
  - 没有命名空间的时候，所有的actions最后都在根actions上面了。所有的mutations都到根mutations上面了。
  - 组件中
  ```js
  ...mapState({
    key:'key',
    key2:state=>state.moduleA.key2
  })
  ```
  ```js
  ...mapGetters(['key1','key2'])
  // key1是根getters里面的，key2是模块getters里面的
  ```
  ```js
  $store.commit('任意模块的mutation方法',实参);
  ...mapMutations(['FN1','FN2']) 
  // FN1是根mutations里面的方法，FN2是模块里面的mutations方法
  ```
  ```js
  $store.dispatch('任意模块的actions里面的方法',实参)
  ...mapActions(['fn1','fn2'])
  // fn1是根actions里面的方法，fn2是模块里面的actions方法
  ```
  - 模块中(actions里面的方法)调用其他模块中的mutations或actions方法
  ```js
  // a模块
  mutations:{
    FN(state,data){
      state.key=data;
    }
  },
  actions:{
    fn(store.info){
      // 触发自己的mutaion方法FN
      store.commit('FN',info);
      // 触发自己的actions里面的其他方法
      store.dispatch('fn2');
      
      // a模块触发b模块里面的方法
      store.commit('FN3',info);
      store.dispatch('fn4');
    },
    fn2(store,info){...}
  }
  ```
   ```js
  // b模块
  mutations:{
    FN3(state,data){
      state.key=data;
    }
  },
  actions:{
    fn3(store.info){
      // 触发自己的mutaion方法FN3
      store.commit('FN3',info)
      // 触发自己的actions里面的其他方法
      store.dispatch('fn4')
    },
    fn4(store,info){...}
  }
   ```
+ 命名空间：**namespaced:true**  📚
  - 组件中
  ```js
  ...mapState({
    key:'key',
    key2:state=>state.moduleA.key2
  })
  ```
  ```js
  ...mapGetters({
    key1:'key1',           // key1是根getters里面的
    key2:'moduleB/key2'    // key2是模块getters里面的key2
  })
  ```
  ```js
  $store.commit('moudulB/moduleB的mutation方法',实参);
  ...mapMutations({
    FN1:'FN1',              // FN1是根mutations里面的方法
    FN2:'moduleB/FN2'      // FN2是模块里面的mutations方法
  }) 
  ```
  ```js
  $store.dispatch('moudulB/moduleB的actions里面的方法',实参)
  ...mapActions({
    fn1:'fn1',             // fn1是根actions里面的方法
    fn2:'moduleB/fn2'      // fn2是模块里面的actions方法
  })
  ```
  - 模块中(actions里面的方法)调用其他模块中的mutations或actions方法
  ```js
  // a模块
  mutations:{
    FN(state,data){
      state.key=data;
    }
  },
  actions:{
    fn(store.info){
      // 触发自己的mutaion方法FN
      store.commit('FN',info);
      // 触发自己的actions里面的其他方法
      store.dispatch('fn2');
      
      // a模块触发b模块里面的方法
      store.commit('moduleB/FN3',info,{root:true});
      store.dispatch('moduleB/fn4',info,,{root:true});
    
      // store 相关信息
         // store.state 当前模块的state
         // store.getters 当前模块的getters
         // store.commit 调用任意模块的mutations,注意模块名,root:true
         // store.dispatch 调用任意模块的actions,注意模块名,root:true
         // store.rootStatte   根state
         // store.rootGetters  根getters
    },
    fn2(store,info){...}
  }
  ```
   ```js
  // b模块
  mutations:{
    FN3(state,data){
      state.key=data;
    }
  },
  actions:{
    fn3(store.info){
      // 触发自己的mutaion方法FN3
      store.commit('FN3',info)
      // 触发自己的actions里面的其他方法
      store.dispatch('fn4')
    },
    fn4(store,info){...}
  }
   ```
------
### 8.7.3 🚀 Vue开发中如何使用插件?

+ ① 安装插件
+ ② 使用ES6的`import...from...`语法或CommonJS的`require()`方法引入插件。

+ ③ 使用全局方法**`Vue.use(plugin)`**使用插件，可以传入一个可选的选项对象来配置插件。需要在你调用 `new Vue()` 启动应用之前完成。

  ```js
  Vue.use(MyPlugin, { someOption: true })
  ```

#### 8.7.3.1 Vue中如何使用jQuery？

+ CDN引入：在`index.html`中通过CDN引入，之后可以在任何组件中直接使用。

+ npm安装后使用。

  ```bash
  npm i jquery -S
  ```

  ```js
  // 在有需求的组件中使用
  import $ from "jquery"
  ```

  

------
## 8.8 常用API

### 8.8.1 实例property

+ **vm.$mount("#el")** ：和el的作用一致。
	
	- 如果Vue实例在实例化时，没有收到el选项，没有关联到DOM元素，可以手动使用这个方法挂载实例。
	
+ **vm.$destroy()** : 主动消亡某个实例。

+ **vm.$delete(target,key)**: 删除对象的某个属性，让页面响应。

+ **vm.$set(target,key,val)**: 添加属性，修改数组，让页面响应。

+ **vm.$el**: 挂载的DOM节点。

+ **vm.$data**: data对象。

+ **vm.$attrs**: vue实例（组件）上的属性。

+ **vm.$watch(变量,函数,{配置对象})**：watch侦听器。

  ```js
  //浅监听
  vm.$watch('num',(newVal,oldVal)=>{
    console.log(newVal,oldVal)
  });
  ```

  ```js
  // 深监听
  vm.$watch('object',(newVal)=>{
    console.log(newVal)
  },{
    deep:true
  })
  ```
  
+ **vm.$forceUpdate()**:强制刷新，迫使Vue 实例重新渲染。注意它仅仅影响实例本身和插入插槽内容的子组件，而不是所有子组件。

+ **vm.$nextTick()** 🚀: 将想要执行的回调延迟到DOM 更新之后再执行。如果直接在修改数据后，执行从渲染出来的DOM上获取数据是不合理的，因为此时DOM还没有更新。所以不可能做到在修改数据后并且DOM更新后再执行，要保证在DOM更新以后再执行某一块代码，就必须把这块代码放到下一次事件循环里面，比如setTimeout(fn, 0)，这样DOM更新后，就会立即执行这块代码。类似于`updated`。

  ```html
  <div id='app'>
     <p id="prag">{{msg}}</p>
     <button @click=changeMsg>改变Msg</button>
  </div>
  
  <script>
  let vm = new Vue({
     el: "#app",
     data: {
       msg: 'hi'
     },
     methods: {
       changeMsg() {
         // 修改数据
         this.msg = "Hello!"
         /** 这里如果直接想要从dom上获取信息，获取的是个更改前的信息
         let p = document.getElementById('prag')
         console.log(p.innerHTML) //hi 
         **/
         Vue.nextTick(() => {
           let p = document.getElementById('prag')
           console.log(p.innerHTML)
         })
       }
     }
  });
  ```

  > 修改数据之后，vm对象上的数据确实是改变了，但是绑定到dom上的数据不能立刻就更新过来的。加上nextTick的原因是保证dom上的元素更新并且渲染完成之后再去执行某个（些）函数，这个（些）函数主要用于跟计算跟dom元素相关的，比如dom上的width,height,innerHTML等等，如果不在nexTtick里面运行的话，你根本不能保证dom里的width,height,innerHTML已经更新并且渲染完成。
------
### 8.8.2 特殊attribute
+ **ref**: 被用来给元素或子组件注册<span id="ref">引用信息</span>。引用信息将会注册在父组件的 $refs 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例：
```vue
<template>
<!-- `vm.$refs.p` will be the DOM node -->
  <p ref="p">hello</p>
</template>
<script>
 new Vue({
   //..
   mounted(){
     this.$refs.p.innerHTML='Hello World'
   }
 })
</script>
```

+ **is**：用于动态组件且基于 DOM 内模板的限制来工作。
# 9. 浏览器

### 9.1 浏览器的渲染过程？

+ 浏览器接收到HTML文件并转换为DOM树。
+ CSS文件下载完后构建CSS树。
+ 将CSS树和DOM树组合构建成渲染树。
+ 布局（layout）：计算出每个节点在屏幕中的位置。
+ 显示（painting）：通过显卡把页面画到 屏幕上。

------
### 9.2 DOM树和渲染树有什么区别？
+ DOM 树与 HTML 标签一一对应，包括 head 和隐藏元素。
+ 渲染树不包括 head 和隐藏元素，大段文本的每一个行都是独立节点，每一个节点都有对应的 css 属性。
------

### 9.3 什么情况下会阻塞渲染？

+ 首先，渲染的前提是生成渲染树，所以HTML和CSS肯定会阻塞渲染。如果你想渲染的越快，你月应该降低一开始需要渲染的文件的大小，并且扁平层级，优化选择器。
+ 浏览器在解析到\<script>标签时，会暂停构建DOM。完成后才会从暂停的地方重新开始，也就是说，如果想首屏渲染的快，首屏就应该尽可能少地加载js文件，这也是都建议将\<script>标签放在\<body>标签底部的原因。
+ 当然，\<script>标签并不一定必须放在底部，因为可以给它加**`defer`或者`async`**属性。当\<script>标签加上`defer`属性后，表示该js文件会并行下载，但是会放到HTML解析完成后顺序执行;对于没有任何依赖的js文件可以加上`async`属性，表示js文件下载和解析不会阻塞渲染。
------
### 9.4 CSS会阻塞DOM解析吗？
+ 对于一个HTML文档来说，不管是内联还是外链的css，都会阻碍后续的dom渲染，但是不会阻碍后续dom的解析。当css文件放在中时，虽然css解析也会阻塞后续dom的渲染，但是在解析css的同时也在解析dom，所以等到css解析完毕就会逐步的渲染页面了。
------

### 9.5 什么是重绘（Repaint）和回流（Reflow）？

+ 重绘：当渲染树中的元素**外观**（如：颜色）发生改变，不影响布局时，产生重绘。
+ 回流：当渲染树中的元素的**布局**（如：尺寸、位置、隐藏/状态状态）发生改变时，产生重绘回流。
+ 注意：JS 获取 Layout 属性值（如：offsetLeft、scrollTop、getComputedStyle 等）也会引起回流。因为浏览器需要通过回流计算最新值
+ 回流必将引起重绘，而重绘不一定会引起回流。回流所需的成本比重绘要高的多。

-------
### 9.6 如何触发回流？
+ 添加或删除可见的DOM元素。
+ 元素位置改变。
+ 元素的尺寸改变（包括：内外边距、边框厚度、宽度、高度等属性的改变）。
+ 内容改变。
+ 页面渲染器初始化。
+ 浏览器窗口尺寸改变。

-------
### 9.7 如何减少重绘和回流？
+ 以下几个操作会导致性能问题：
	- 改变 window 大小
	- 改变字体
	- 添加或删除样式
	- 文字改变
	- 定位或者浮动
	- 盒模型
+ 解决方法：
	- 需要要对DOM元素进行复杂的操作时，可以先隐藏(display:"none")，操作完成后再显示
	- 需要创建多个 DOM 节点时，使用 DocumentFragment 创建完后一次性的加入 document，或使用字符串拼接方式构建好对应HTML后再使用innerHTML来修改页面
缓存 Layout 属性值，如：var left = elem.offsetLeft; 这样，多次使用 left 只产生一次回流
	- 避免用 table 布局（table 元素一旦触发回流就会导致 table 里所有的其它元素回流）
	- 避免使用 css 表达式(expression)，因为每次调用都会重新计算值（包括加载页面）
	- 尽量使用 css 属性简写，如：用 border 代替 border-width, border-style, border-color
	- 批量修改元素样式：elem.className 和 elem.style.cssText 代替 elem.style.xxx

------

### 9.8 说说从输入URL到看到页面发生的全过程？

+ ① 输入网址
  - 当你开始输入网址比如www.cnblogs.com时游览器就可以在书签或者历史记录里面去搜索相关的网址推荐给你。
+ ② 游览器查找域名的IP地址
  - 请求发起后，游览器首先会解析这个域名，首先它会查看本地硬盘的 hosts 文件，看看其中有没有和这个域名对应的规则，如果有的话就直接使用 hosts 文件里面的 ip 地址。

  - **如果在本地的 hosts 文件没有能够找到对应的 ip 地址，浏览器会发出一个 DNS请求到本地DNS(域名分布系统)服务器** 。本地DNS服务器一般都是你的网络接入服务器商提供，比如中国电信，中国移动。

  - 查询你输入的网址的DNS请求到达本地DNS服务器之后，本地DNS服务器会首先查询它的缓存记录，如果缓存中有此条记录，就可以直接返回结果，此过程是递归的方式进行查询。如果没有，本地DNS服务器还要向DNS根服务器进行查询

  - 根DNS服务器没有记录具体的域名和IP地址的对应关系，而是告诉本地DNS服务器，你可以到域服务器上去继续查询，并给出域服务器的地址。这种过程是迭代的过程。

  - 本地DNS服务器继续向域服务器发出请求，在这个例子中，请求的对象是.com域服务器。.com域服务器收到请求之后，也不会直接返回域名和IP地址的对应关系，而是告诉本地DNS服务器，你的域名的解析服务器的地址。

  - 最后，<u>本地DNS服务器向域名的解析服务器发出请求，这时就能收到一个域名和IP地址对应关系，本地DNS服务器不仅要把IP地址返回给用户电脑，还要把这个对应关系保存在缓存中，以备下次别的用户查询时，可以直接返回结果，加快网络访问</u>。
+ ③ 建立TCP连接
  - 在拿到域名对应的IP地址后，会以随机端口（1024~~65535）向WEB服务器程序80端口发起TCP的连接请求，这个连接请求进入到内核的TCP/IP协议栈（用于识别该连接请求，解封包，一层一层的剥开），还有可能要经过Netfilter防火墙（属于内核的模块）的过滤，最终到达WEB程序，最终建立了TCP/IP的连接，对于客户端与服务器的TCP链接，必然要说的就是『三次握手』。
  - 客户端发送一个带有SYN标志的数据包给服务端，服务端收到后，回传一个带有SYN/ACK标志的数据包以示传达确认信息，最后客户端再回传一个带ACK标志的数据包，代表握手结束，连接成功。
+ ④ 游览器向WEB服务器发起Http请求
  - 建立TCP连接之后，发起HTTP请求，请求一般分为三部分
    + 请求方法URI协议/版本
    + 请求头(Request Header)
    + 请求正文
+ ⑤ 服务器端处理
  - 服务器端收到请求后的由web服务器（准确说应该是http服务器）处理请求，诸如Apache、Ngnix、IIS等。web服务器解析用户请求，知道了需要调度哪些资源文件，再通过相应的这些资源文件处理用户请求和参数，并调用数据库信息，最后将结果通过web服务器返回给浏览器客户端。
+ ⑥ 关闭TCP链接
  - 为了避免服务器与客户端双方的资源占用和损耗，当双方没有请求或响应传递时，任意一方都可以发起关闭请求。与创建TCP连接的3次握手类似，关闭TCP连接，需要**4次握手**。
+ ⑦ 浏览器解析资源
  - 浏览器通过解析HTML，生成DOM树，解析CSS，生成CSS规则树，然后通过DOM树和CSS规则树生成渲染树。渲染树与DOM树不同，渲染树中并没有head、display为none等不必显示的节点。
  - 在解析CSS的同时，可以继续加载解析HTML，但在解析执行JS脚本时，会停止解析后续HTML，这就会出现阻塞问题。
+ ⑧ 浏览器布局渲染
  - 根据渲染树布局，计算CSS样式，即每个节点在页面中的大小和位置等几何信息。HTML默认是流式布局的，CSS和js会打破这种布局，改变DOM的外观样式以及大小和位置。
  - 布局结束后，通过GPU把页面输出到显示器上。
> 详细参考：[从输入URL到看到页面发生了什么？](https://www.cnblogs.com/yuanzhiguo/p/8119470.html)
------

### 9.6 安全防范

#### 9.6.1 什么是XSS攻击？如何防范XSS攻击？
> 人们经常将跨站脚本攻击（Cross Site Scripting）缩写为CSS，但这会与层叠样式表（Cascading Style Sheets，CSS）的缩写混淆。因此，有人将跨站脚本攻击缩写为XSS。
+ XSS(Cross Site Scripting)简单点来说就是攻击者详尽一切办法将可以执行的代码注入到网页中。XSS可以分为多种类型，但是总体上分为两类：持久型和非持久型。
+ 持久型就是攻击的代码被服务器写入了数据库，这种攻击危害性很大，因为如果网站访问量很大时，就会导致大量正常访问页面的用户都受到攻击。
+ 非持久型相比前者尾号就小很多，一般通过修改URL参数的方式加入攻击代码，诱导用户访问链接从而进行攻击。
+ 对于XSS攻击来说，通常有两种方式可以用来防御：
  - 转义字符
    
  - CSP

------

#### 9.6.2 什么是CSP？
+ CSP（Content-Security-Policy）即**内容安全策略**。开发者明确告诉浏览器哪些外部资源可以加载和执行。我们只需要配置规则，如何拦截是由浏览器自己实现的，我们可以通过这种方式来尽量减少XSS攻击。


+ 通常有两种方式来开启CSP：
  - 设置`meta`标签的方式
  ```html
  <meta http-equiv="Content-Security-Policy">
  ```
  - 设置HTTP 头部 中的 `Content-Security-Policy`.
    + 只允许加载本站资源：
    ```http
    Content-Security-Policy:default-src 'self'
    ```
    + 只允许加载HTTPS协议图片
    ```http
    Content-Security-Policy:img-src https://*
    ```
    + 允许加载任何来源框架
    ```http
    Content-Security-Policy:child-src 'none'
    ```
> 更多资料，参考：[MDN:CSP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CSP)
------

#### 9.6.3 什么是CSRF攻击？如何防范CSRF攻击？

+ **CSRF**（Cross-Site Request Forgery）跨站请求伪造,是一种冒充受信任用户，向服务器发送非预期请求的攻击方式。
+ 

------

#### 9.6.4 什么是点击劫持？如何防范点击劫持？

+ 点击劫持是一种视觉欺骗的攻击手段。攻击者将需要攻击的网站通过iframe 嵌套的方式嵌入自己的网页中，并将iframe 设置为透明，在页面中透出一个按钮诱导用户点击。

-------
### 9.7 浏览器缓存
+ 浏览器的缓存机制指的是通过在一段时间内保留已接收到的 web 资源的一个**副本**，如果在资源的有效时间内，发起了对这个资源的再一次请求，那么浏览器会直接使用缓存的副本，而不是向服务器发起请求。使用 web 缓存可以有效地提高页面的打开速度，减少不必要的网络带宽的消耗。web 资源的缓存策略一般由服务器来指定，可以分为两种，分别是**强缓存策略**和**协商缓存策略**。

+ 强缓存
  - 使用**强缓存策略**时，如果缓存资源有效，则直接使用缓存资源，不必再向服务器发起请求。强缓存策略可以通过两种方式来设置，分别是 http 头信息中的 `Expires` 属性和 `Cache-Control` 属性。
  - 服务器通过在响应头中添加 `Expires` 属性，来指定资源的过期时间。在过期时间以内，该资源可以被缓存使用，不必再向服务器发送请求。这个时间是一个绝对时间，它是服务器的时间，因此可能存在这样的问题，就是客户端的时间和服务器端的时间不一致，或者用户可以对客户端时间进行修改的情况，这样就可能会影响缓存命中的结果。`Expires` 是 http1.0 中的方式，因为它的一些缺点，在 http 1.1 中提出了一个新的头部属性就是 Cache-Control 属性，它提供了对资源的缓存的更精确的控制。它有很多不同的值，常用的比如我们可以通过设置 max-age 来指定资源能够被缓存的时间的大小，这是一个相对的时间，它会根据这个时间的大小和资源第一次请求时的时间来计算出资源过期的时间，因此相对于 Expires来说，这种方式更加有效一些。常用的还有比如 private ，用来规定资源只能被客户端缓存，不能够代理服务器所缓存。还有如 no-store ，用来指定资源不能够被缓存，no-cache 代表该资源能够被缓存，但是立即失效，每次都需要向服务器发起请求。一般来说只需要设置其中一种方式就可以实现强缓存策略，当两种方式一起使用时，`Cache-Control` 的优先级要高于` Expires` 。

+ 协商缓存
  - 使用**协商缓存策略**时，会先向服务器发送一个请求，如果资源没有发生修改，则返回一个 304 状态，让浏览器使用本地的缓存副本。如果资源发生了修改，则返回修改后的资源。协商缓存也可以通过两种方式来设置，分别是 http 头信息中的 Etag 和 Last-Modified 属性。
  - 服务器通过在响应头中添加 Last-Modified 属性来指出资源最后一次修改的时间，当浏览器下一次发起请求时，会在请求头中添加一个 If-Modified-Since 的属性，属性值为上一次资源返回时的 Last-Modified 的值。当请求发送到服务器后服务器会通过这个属性来和资源的最后一次的修改时间来进行比较，以此来判断资源是否做了修改。如果资源没有修改，那么返回 304 状态，让客户端使用本地的缓存。如果资源已经被修改了，则返回修改后的资源。使用这种方法有一个缺点，就是 Last-Modified 标注的最后修改时间只能精确到秒级，如果某些文件在1秒钟以内，被修改多次的话，那么文件已将改变了但是 Last-Modified 却没有改变，
这样会造成缓存命中的不准确。
  - 因为 Last-Modified 的这种可能发生的不准确性，http 中提供了另外一种方式，那就是 Etag 属性。服务器在返回资源的时候，在头信息中添加了 Etag 属性，这个属性是资源生成的唯一标识符，当资源发生改变的时候，这个值也会发生改变。在下一次资源请求时，浏览器会在请求头中添加一个 If-None-Match 属性，这个属性的值就是上次返回的资源的 Etag 的值。服务接收到请求后会根据这个值来和资源当前的 Etag 的值来进行比较，以此来判断资源是否发生改变，是否需要返回资源。通过这种方式，比 Last-Modified 的方式更加精确。当 Last-Modified 和 Etag 属性同时出现的时候，Etag 的优先级更高。使用协商缓存的时候，服务器需要考虑负载平衡的问题，因此多个服务器上资源的 Last-Modified 应该保持一致，因为每个服务器上 Etag 的值都不一样，因此在考虑负载平衡时，最好不要设置 Etag 属性。

+ `强缓存策略`和`协商缓存策略`在缓存命中时都会直接使用本地的缓存副本，区别只在于`协商缓存`会向服务器发送一次请求。它们缓存不命中时，都会向服务器发送请求来获取资源。在实际的缓存机制中，`强缓存策略`和`协商缓存策略`是一起合作使用的。浏览器首先会根据请求的信息判断，`强缓存`是否命中，如果命中则直接使用资源。如果不命中则根据头信息向服务器发起请求，使用`协商缓存`，如果`协商缓存`命中的话，则服务器不返回资源，浏览器直接使用本地资源的副本，如果`协商缓存`不命中，则浏览器返回最新的资源给浏览器。

> 详细资料可以参考：
[《浅谈浏览器缓存》](https://segmentfault.com/a/1190000012573337)
[《前端优化：浏览器缓存技术介绍》](https://juejin.im/post/5b9346dcf265da0aac6fbe57#heading-3)
[《请求头中的 Cache-Control》](https://www.web-tinker.com/article/21221.html)
[《Cache-Control 字段值详解》](https://juejin.im/post/5c2d6c9ae51d450cf4195a08)
-------

# 10. 简单算法

## 10.0 算法评价
+ 时间复杂度：算法实现的时间度量。
  - **O(1)** 表示执行时间恒定的算法，与问题的规模无关，称之为具有O(1)的时间复杂度。
  - O(1)< O(log*n*)<O(*n*)<O(*n*log*n*)<O(n^2^)<O(n^3^)<O(2^n^)<O(*n*!)<O(n^n^)

+ 空间复杂度：算法执行时所需的辅助空间（临时变量占用空间）的大小的度量。
  - 若算法执行时所需的辅助空间相对于输入数据量而言是个常数，则称此算法为原地工作，空间复杂度为**O(1)**。

------
## 10.1 排序算法


------
### 10.1.1 🔥冒泡排序

+ 冒泡排序原理： 从第一个元素开始，把当前元素和下一个索引元素进行**比较**，如果当前元素比较大，就**交换**位置，重复操作直到最后一个元素，那么此时最后一个元素就是该数组中最大的数。下一轮重复以上操作，但是此时最后一个元素已经是最大值了，所以不需要再比较最后一个元素，只需要比较到`length-1`的位置。

<div align="center">
  <img src="./0_pictures/bubbleSort.gif" alt="bubbleSort" />
</div>


> 关键：一共遍历arr.length-1轮，第i轮需要【比较判断是否交换】arr.length-i-1次

```js
function bubbleSort(arr) {
  if (!Array.isArray(arr)) return;
  for (let i = 0; i < arr.length - 1; i++) {
    for (let j = 0; j < arr.length - i - 1; j++) {
      if (arr[j] >= arr[j + 1]) {
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
      }
    }
  }
  return arr;
}
// test
let arr = [1, 2, -1, 4, 0, 9, 3, 2]
let result = bubbleSort(arr);
console.log(result);
```

+ 算法分析：(n=arr.length)
  时间复杂度：**O(n^2^)**
  
  ```mathematica
  (*最差: 每次比较都需要交换顺序，每次的交换需要3次记录*)
  3*[(n-1)+(n-2)+...+1] = 3n(n-1)/2 ~ O(n^2)
  (*最优: 每次比较都不需要交换顺序*)
  (n-1)+(n-2)+...+1 = n(n-1)/2 ~ O(n^2)
  ```
  
  空间复杂度：**O(n)**
  
  ```mathematica
  (*最差:完全逆序，每轮遍历都需要一个临时变量，一共需要n-1*)
  n-1 ~ O(n)
  (*最优:完全正序，不需要为了交换而建立临时变量,也就是不需要借用第三方内存空间*)
  0 ~ O(1)
  ```

+ 算法优化：
  
  + 针对每轮遍历，设置标志位，如果本轮循环只是发生了“比较”，而未发生”交换“，那么这轮遍历结束后已经排好序，可以退出循环。
```js
function bubbleSortBetter(arr) {
  if (!Array.isArray(arr)) return;
  for (let i = 0; i < arr.length - 1; i++) {
    let flag = false;
    for (let j = 0; j < arr.length - i - 1; j++) {
      if (arr[j] >= arr[j + 1]) {
        let temp = arr[j];
        arr[j] = arr[j + 1];
        arr[j + 1] = temp;
        flag = true;
      }
    }
    if(!flag) break;
  }
  return arr;
}
```

------

### 10.1.2 🔥选择排序

+ 首先找到从第二个元素开始的数组中的最小元素，将它和数组的第一个元素比较，如果第一个元素较大，就交换位置。然后在第三个元素开始的数组中找到最小的元素，将它与数组的第二个元素交换比较，如果第二个元素较大，就交换位置。以此类推，直到数组的最后一个元素（此时已没有与之对应的比较数组）。这种方法叫做**选择排序**，因为它在不断地选择剩余元素之中的最小者。
<div align="center">
  <img src="./0_pictures/selectionSort.gif" alt="selectionSort1" />
</div>
> 关键:一共遍历n-1轮（当剩余元素就是最后一个元素时，不再需要对其遍历），第i轮遍历的“剩余元素”有n-i+1个。
```js
function selectionSort(arr) {
    for (let i = 0; i < arr.length - 1; i++) {
      // 不断地假设当前遍历数组的第一项就是最小值
      let currentMin = arr[i], currentMinIndex = i;
      for (let j = i + 1; j < arr.length; j++) {
        if (currentMin > arr[j]) {
          currentMin = arr[j];
          currentMinIndex = j;
        }
      }
      if (currentMinIndex != i) {// 需要交换！
        arr[currentMinIndex] = arr[i];
        arr[i] = currentMin;
      }
    }
    return arr;
  }
let arr = [1, 2, -1, 4, 0, 9, 3, 2]
let result = selectionSort(arr);
console.log("选择排序:"+result);
```
+ 算法分析：
  
  - 时间复杂度：**O(n^2^)** ：运行时间和输入（有序度）无关。它不善于利用输入的初始状态（初始有序性）。
  ```mathematica
  (*最差: 第一次需要比较n-1次，第二次n-2...; 交换次数最多n-1次*)
  (n-1)+(n-2)+...+1 = n(n-1)/2 ~ O(n^2)
  (*最优: 每次比较都不需要交换顺序，但依然需要完成全部比较*)
  (n-1)+(n-2)+...+1 = n(n-1)/2 ~ O(n^2)
  ```
  - 空间复杂度：**O(n)**: 数组移动最少，其他任何算法都不具备此特征。
  ```mathematica
  (*最差: 交换次数最多n-1次*)
  1+1+...+1 = n-1 ~ O(n)
  (*最优: 每次比较都不需要交换顺序，但依然需要完成全部比较*)
  0+0+...+0 = 0 ~ O(1)
  ```
-------

### 10.1.3 🔥插入排序

+ 从第一个元素开始，认为该元素已经被排序。取出下一个元素作为“当前元素”，与左边已经排好的序列逐个从后往前扫描，如果当前元素小于前一个元素就再往前扫描，直到直到”当前元素“大于上一个元素，此时就可以将”当前元素“移位至”上一个元素“的下一个位置。
<div align="center">
  <img src="./0_pictures/insertionSort.gif" alt="insertionSort1" />
</div>

> 关键：从index为1的第一个元素开始，将其取出，逐步和前面已经排好的序列尝试插入逐个对比，直到找到一个大于前一个元素的位置后插入。

```js
function insertionSort(arr) {
    let preIndex, current
    for (let i = 1; i < arr.length; i++) {
      current = arr[i];
      // 从当前元素的前一项逐步往前对比迭代，直至下标为0的第一个元素停止
      // 前面的元素大于当前元素要后移
      for (preIndex = i - 1; preIndex >= 0 && arr[preIndex] > current; preIndex--) {
        // preIndex+1 表示当前元素的原始位置，这个位置被赋予了前一个元素的值，表示前一个元素后移一位。
        arr[preIndex + 1] = arr[preIndex];
      }
      arr[preIndex + 1] = current;
    };
    return arr;
  }
let arr = [1, 2, -1, 4, 0, 9, 3, 2]
let result = insertionSort(arr);
console.log("插入排序:" + result);
```
+ 算法分析：
  - 时间复杂度：**O(n^2^)**: 第一次插入比较一次，第二次比较二次。。。
  ```mathematica
  1+2+3+...+(n-1) = n(n-1)/2 ~ O(n^2)
  ```
  - 空间复杂度：**O(1)**: 插入排序只使用了i和preIndex这两个辅助元素，与问题规模无关。
------

### 10.1.4 希尔排序



------

### 10.1.5 归并排序



------

### 10.1.6 快速排序



--------

### 10.1.7 堆排序



------

### 10.1.8 计数排序



------

### 10.1.9 桶排序



------

###10.1.10 基数排序





> 待补充动图：[博客园-一像素-十大经典排序算法](https://www.cnblogs.com/onepixel/articles/7674659.html)

------

## 10.2 链表



------

## 10.3 树



------

## 10.4 背包、队列和栈



------

## 10.5 图

