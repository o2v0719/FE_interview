# typescript 基础

## 1. 环境搭建

- 工具 1：typescript 运行环境

```bash
sudo npm install -g typescript
```

> 安装成功后，命令行使用 **tsc+文件名** 即可编译 ts 文件。

- 工具 2：编译 typescript 并执行编译后的 javascript

```bash
sudo npm install -g ts-node
```

> 安装成功后，命令行使用 **ts-node+文件名** 即可编译并执行。

## 2. TS 类型的类型概念

### 2.1 ts 类型有哪些

- 基础类型有：boolean， number，string，void，undefined，symbol，null
- 对象类型：object，class，function，array
- 任意类型：any

```ts
// 基础类型
const count: number = 1
const teacherName: string = 'Dell'
// 常规对象
const teacher: {
  name: string
  age: number
} = {
  name: 'Dell',
  age: 18,
}
// 由数字构成的数组
const arr: number[] = [1, 2, 3]
// 类
class Person {}
const xiaoming: Person = new Person()
// 函数：限制函数的返回值类型
const getTotal: () => number = () => {
  return 123
}
```

### 2.2 类型注解和类型推断

- 类型注解：我们来告诉 ts 变量是什么类型。

```ts
let count: number
count = 22
```

- 类型推断：给没有类型的变量赋值后会自动推断变量的类型。如果 ts 能自动分析变量类型，我们就什么也不许要做了。如果 ts 无法分析变量类型，我们就需要使用类型注解。

```ts
let firstNum: number = 1
let secondNum: number = 2
// 类型推断total为number类型
const total = firstNum + secondNum

// 类型推断date为Date类型！
const date = new Date()
```

### 2.3 函数类型的特点

- 声明了**void**表示没有返回值

```ts
function sayHello(): void {
  console.log('Hello')
}
```

- 声明了**never**表示函数永远不可能执行到最后

```ts
function errorFunc(): never {
  while (true) {}
}
```

- 我们可以给函数的每个参数指定类型，并为函数本身指定返回值类型，Typescript 能够根据返回语句自动推断出返回值类型，因此我们通常省略它。

> 在某些项目实际开发中，需要明确指定函数的返回值类型，可以避免返回语句引发的类型错误。

```ts
let getTotal = (firstNum: number, secondNum: number): number => {
  return firstNum + secondNum
}
```

- 上面给出了一种**书写完整函数类型**的方法，还有一种写法

```ts
let myAdd: (firstNum: number, secondNum: number) => number = (
  firstNum,
  secondNum
) => {
  return firstNum + secondNum
}
```

- 声明类型可以使用“|”,表示不确定类型

```ts
let temp: number | string = 123
temp = '456'
```

- 其他 case

```ts
interface Person {
  name: 'string'
}
const rawData = '{"name":"Dell"}'
// 这里必须声明newData 的类型
const newData: Person = JSON.parse(rawData)
```

### 2.4 数组与元组

- 声明类型数组的方式：

```ts
// 声明常规数组（单一number类型）
const numberArr: number[] = [1, 2, 3]
// 声明多种元素类型的数组
const arr: (number | string)[] = [1, 'g', 3]
// 声明undefined类型的数组
const undefinedArr: undefined[] = [undefined]
```

- 类型别名 type alias

```ts
const objArr: { name: string; age: number }[] = [{ name: 'Jack', age: 18 }]

// 使用type类型别名，可以避免上述臃肿的声明结构
type User = { name: string; age: number }
const objectArr: User[] = [
  {
    name: 'Dell',
    age: 25,
  },
]


class Teacher{
  name:string,
  age:number
}
const teacherArr:Teacher[] = [
  new Teacher(),
  {
    name:'Merry',
    age:23
  }
]
```

- **元组** tuple： 元组类型允许表示一个**已知元素数量和类型**的数组，各元素的类型不必相同.

* 元组可以更加**准确地约束数组**。

```ts
const teacherInfo: [string, string, number] = ['Dell', 'male', 19]

// 以元组为单位的数组
const teacherList: [string, string, number][] = [
  ['Lisa', 'female', 24],
  ['Jason', 'male', 30],
]
// 可以使用数组的方法添加符合限制类型的元素
let user:[string , number] = ['viking',20]
user.push(30)
console.log(user)// ['viking',20,30]
```

### 2.5 枚举 enum

- **枚举**的作用是列举类型中包含的各个值。是一种**无序**数据结构。

* 使用**枚举**可以定义一些带名字的常量。使用枚举可以**清晰地表达意图或创建一组有区别的用例**。 TypeScript 支持数字的和基于字符串的枚举。

- 枚举支持**反向取值**。

```ts
// 枚举类型
enum Status {
  OFFLINE,
  ONLINE,
  DELETED,
}

function getResult(status) {
  if (status === Status.OFFLINE) {
    return 'offline'
  } else if (status === Status.ONLINE) {
    return 'online'
  } else if (status === Status.DELETED) {
    return 'deleted'
  }
  return 'error'
}
const result = getResult(Status.OFFLINE)
console.log(result)

console.log(Status.OFFLINE) //0

// 反向取值
console.log(Status[0]) //OFFLINE
```

#### 2.5.1 数字枚举

- TS 可以自动为数字枚举中的各个成员推导对应的数字。

```ts
enum Direction {
  Up = 1,
  Down, //2
  Left, //3
  Right, //4
}
```

- 第一个成员 Up 使用初始化为 1。 其余的成员会从 1 开始自动增长。
- 如果不初始化，第一个成员及其余成员从 0 开始增长。

#### 2.5.2 字符串枚举

```ts
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT',
}
```

- 在一个字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化。
- 由于字符串枚举没有自增长的行为，字符串枚举可以很好的序列化。

#### 2.5.3 常量枚举

```ts
const enum  Direction {
	Up='UP',
	Down='DOWN',
	Left='Left',
	Right='Right',
}
const value = 'UP'
// 调用枚举，不会把枚举编译成js代码。可提升性能
if(value===Direction.Up){  // 编译成 if(value==='UP')
	console.log('go up!')
}
```

## 3. 接口 interface

- **接口**可以给类型化的结构命名 ( 用来描述对象），它可以用来定义一种通用类型，来限制**对象**和**函数**。

* 注意接口里面的不同的键值对结构使用分号;隔开。

### 3.1 interface 和 type 的区别

- interface 和 type 类似，但并不完全一致。
  
+ 相同点：
  - 1. 都可以描述一个对象或函数的结构
  - 2. 都可以使用`extends`或`&`关键字拓展。

+ 不同点：

  - 1. type 可以声明基本类型别名，联合类型，元祖等类型；interface只能声明对象或函数的结构。
  - 2. 扩展接口`interface`时，TS将会检查扩展的接口是否可赋值给被扩展的接口。
  - 3. `interface`的同名声明可以合并，而`type`类型别名重写的话，会报错。
  - 4. `type`语句中还可以使用`typeof`获取实例的类型进行赋值。

> 参考: [掘金:TS中interface和type到底有什么区别?]


------
### 3.2 interface 的使用

```ts
interface Person {
  name: string
  age: number
}
const getPersonName = (person: Person): void => {
  console.log(person.name)
}
const setPersonName = (person: Person, name: string): void => {
  person.name = name
  console.log(person.name)
}

let person = { name: 'Dell', age: 18 }

getPersonName(person) //Dell
setPersonName(person, 'Ted') //Ted
// 在这里Person代表了:有一个name属性，且类型为string;同时有一个age属性，且类型为number的对象。
```

### 3.3 interface 的可选属性

```ts
interface GoodPerson {
  name: string
  // age 属性可选，如果有age属性，其值的类型必须是number
  age?: number
}
const getPersonName = (person: GoodPerson): void => {
  console.log(person.name)
}
const setPersonName = (person: GoodPerson, name: string): void => {
  person.name = name
}
const person = {
  name: 'Dell',
  //  age 不一定存在
}

setPersonName(person, 'Ted')
```

### 3.4 interface 的只读属性

- 在接口里面的属性之前声明**readonly**可以声明该属性为只读属性

```ts
interface Person {
  name: string
  age: number
  readonly gender: 'male'
}
// 只读属性不能再更改
```

### 3.5 规范引用对象和规范对象字面量的区别

- interface 规范**引用对象**时，引用对象的额外属性可以存在。也就是说规范过程是**弱校验**。

```ts
interface Person {
  name: string
  age: number
}
const setPersonName = (person: Person, name: string): void => {
  person.name = name
}
// 要规范的引用对象
const person = {
  name: 'Bob',
  age: '15',
  // 弱校验：允许gender存在
  gender: 'male',
}
setPersonName(person, 'Ted')
// person 被引用，其内部的gender可以存在
```

- interface 规范**对象字面量**，对象字面量的结构要严格和接口保持一致。也就是说规范对象字面量的过程是**强校验** 。

```ts
interface Person {
  name: string
  age: number
}
const setPersonName = (person: Person, name: string): void => {
  person.name = name
  console.log(person.name)
}
setPersonName(
  // 对象字面量
  {
    name: 'Bob',
    age: 15,
    // 这里不能有额外的属性
  },
  'Ted'
)
```

- 针对上述对象字面量的强校验，如何解决？

```ts
interface Person {
  name: string
  age: number
  // 属性名是字符串类型，属性值可以是any类型
  [propName: string]: any
  say(): string
}
const getPersonName = (person: Person): void => {
  console.log(person.name)
}
getPersonName({
  name: 'Bob',
  age: 34,
  gender: 'male',
  say() {
    return 'sayHello'
  },
})
```

### 3.6 让类实现（符合）接口 【implements 关键字】

- implements 强制一个类去实现（符合）接口。接口里面必须存在的属性在类里面也必须存在。类里面可以存在接口里面没有规范的结构。

* class A implements B(interface) , 我们可以称作是**类 A 实现了接口 B**。

- 接口描述了类的公共部分，而不是公共和私有两部分，它不会帮你检查是否具有某些私有成员。

```ts
interface Person {
  name: string
  age: number
}
class User implements Person {
  name: string
  age: 18
  say() {
    return 'hello'
  }
}
user = new User()
console.log(user.say()) // hello
```

### 3.7 interface 接口的扩展 【extends】关键字

- interface 也可以使用关键字**extends**实现扩展（继承）。

```ts
interface Person {
  name: string
  age: number
  say(): string
}
interface Teacher extends Person {
  teach(): string
}

const getTeacherWords = (teacher: Teacher): void => {
  console.log(teacher.say())
  console.log(teacher.teach())
}

teacher1 = {
  name: 'Dell',
  age: 23,
  say() {
    return 'Hello'
  },
  teach() {
    return 'Teach'
  },
}
getTeacherWords(teacher1) // Hello Teach
```

### 3.8 interface 接口用来规范函数

- interface 还可以用来限制规范**函数的类型**

```ts
// 注意参数类型和返回值类型之间用冒号隔开
interface SayHi {
  (word: string): string
}
// 写法1：
const say: SayHi = (word: string) => {
  return word
}
// 写法2：
let say2: SayHi
say2 = (something: string) => {
  return something
}
console.log(say('How Are U?'))
console.log(say2('How Old R U?'))
```

## 4. 类

### 4.1 类中的修饰符

- public , private,protected

* 在 TS 中，类中的所有成员都默认为 public 类。标记为 **public** 的成员，（默认），允许在类的内外被调用。

* 标记为 **private** 的成员，只允许在类内被使用。
  
  > 习惯上，对于声明了 private 的成员，成员命名时在前面加下划线加以区分。
* 标记为 **protected** 的成员，只允许在类内及继承的子类中使用。
  
  > 内指在声明的类中，实例化后使用就是类外。

```ts
class Person {
  protected name: string
  private sayABC() {
    console.log('private')
  }
  public sayHi() {
    console.log(this.name)
    console.log('Hi')
    console.log(this.sayABC())
  }
}

class Teacher extends Person {
  sayBye() {
    console.log(this.name)
    this.sayHi()
  }
}
```

### 4.2 在构造器中使用参数属性来初始化一个类的成员

- 参数属性通过给构造函数参数前面添加一个访问【**限定符**】来声明,可以用来简化类的结构。

```ts
class Person {
  public name: string
  constructor(name: string) {
    this.name = name
  }
}
// 简化写法，参数属性
class NewPerson {
  constructor(public name: string) {
    this.name = name
  }
}
const person = new Person('dell')
const newperson = new NewPerson('DELL')
console.log(person.name)
console.log(newperson.name)
```

### 4.3 **super**关键字实现类的继承

- 子类里面声明了 constructor，必须要使用 super()

```ts
class Person {
  constructor(public name: string) {}
}
class Teacher extends Person {
  constructor(public age: number) {
    super('Dell')
  }
}
const teacher = new Teacher(28)
console.log(teacher.name) //Dell
console.log(teacher.age) //28
```

### 4.4 存取器 set 和 get

- TypeScript 支持通过 getters/setters 来拦截成员的存取行为。 它能帮助你有效的控制对对象成员的访问。

```ts
class Person {
  constructor(private _name: string) {}
  get name() {
    return this._name + '123'
  }
  set name(name: string) {
    this._name = name
  }
}
const person = new Person('Dell')
console.log(person.name) //Dell123      //取
person.name = 'DellLee' // 存
console.log(person.name) //DellLee123   //取
// 访问person.name, 实际上就是要通过取值函数getter访问name属性。
```

### 4.5 \*\*单例模式

- 限制类只能生成换一个实例

* 属性前加**static**关键字，表示静态属性

```ts
// 了解
class Demo {
  private static instance: Demo
  private constructor(public name: string) {}
  static getInstance() {
    if (!Demo.instance) {
      Demo.instance = new Demo('Dell')
    }
    return Demo.instance
  }
}
const demo1 = Demo.getInstance()
const demo2 = Demo.getInstance()
console.log(demo1.name)
console.log(demo2.name)
```

### 4.6 抽象类

- **抽象类**作为其他派生类的基类使用，他们一般不会被实例化，**abstract**关键字是用于定义抽象类和在抽象类内部定义抽象方法

* 抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。

```ts
abstract class Geom {
  getType() {
    return 'Geom'
  }
  // 被“抽象”的方法，必须在子类中得到具体实现
  abstract getArea(...args: [number]): number
}
class circle extends Geom {
  constructor() {
    super()
  }
  getArea(radius: number): number {
    return Math.PI * radius ** 2
  }
}
```

- 不同于接口，抽象类可以包含成员的实现细节。**抽象类只能被继承而不能直接使用**，子类继承抽象类后，抽象类有抽象方法的话，子类必须实现具体的方法。

## 5 进阶语法

### 5.1 联合类型

- **联合类型**表示一个值可以是几种类型之一，我们用竖线分割每个类型。

```ts
function padLeft(value: string, padding: string | number) {
    // ...
}

let indentedString = padLeft("Hello world", 123); /
```

- 如果一个值是**联合类型**，我们只能访问此联合类型的所有类型里**共有**的成员。

```ts
interface Bird {
  fly: boolean
  sing: () => {}
}
interface Dog {
  fly: boolean
  bark: () => {}
}
function trainAnimal(animal: Bird | Dog) {
  animal.fly
}
```

### 5.2 类型保护

- TypeScript 能够在特定的区块中**保护==变量==属于某种确定的类型**，可以在此区块中放心的引用此类型的属性，或者调用此类型的方法。

#### 实现类型保护的几种方式:

- 1. **类型断言实现类型保护**

```ts
interface Bird {
  fly: boolean
  sing: () => {}
}
interface Dog {
  fly: boolean
  bark: () => {}
}
function trainAnimal(animal: Bird | Dog) {
  if (animal.fly) {
    animal.sing()
  } else {
    animal.bark()
  }
}
```

- 2. **in 语法**

```ts
function trainAnimalSecond(animal: Bird | Dog) {
  if ('sing' in animal) {
    animal.sing()
  } else {
    animal.bark()
  }
}
```

- 3. **typeof**

```ts
function add(first: string | number, second: string | number) {
  if (typeof first === 'string' || typeof second === 'string') {
    return `${first}${second}`
  }
  return first + second
}
```

- 4. **instanceof**

> **instanceof** 用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

> 注意：**instanceof**不能调用 interface。

```ts
class NumberObj {
  count: number
}
function addSecond(first: object | NumberObj, second: object | NumberObj) {
  if (first instanceof NumberObj && second instanceof NumberObj) {
    return first.count + second.count
  }
}
```

### 5.3 类型断言

> 有时，我们没有足够的时间把所有的类型都规划好，这时，我们希望Typescript能相信我们，即便如此也是安全的。Typescript提供了**类型断言**。

#### 5.3.1 类型断言的使用场景
+ ① 将一个联合类型断言为其中一个类型。
+ ② 将一个父类（变量的原生父类）断言为更加具体的子类。
+ ③ 将任何一个类型断言为any。
+ ④ any可以被断言为任何类型。

#### 5.3.2 类型断言的句法
+ **as关键字**
```ts
	function formatInput(input:string){
		//...
	}
	function getUserInput():string|number{
		//...
	}
	let input = getUserInput();
	// 类型断言：断定input是字符串
	formatInput(input as string);
```

### 5.4 函数泛型

#### 5.4.1 泛型的概念

- **泛型**generic 泛指的类型。在类型层面施加约束的占位类型，也称多态类型参数。

- 泛型参数使用尖括号<>声明，尖括号的位置限定泛型的作用域，Typescript 将确保当前作用域中相同的泛型参数最终都绑定同一个具体类型。

#### 5.4.2 函数中的泛型

+ 在函数的参数和返回值中使用泛型。

```ts
function join1<ABC>(first: ABC, second: ABC) {
  return `${first}${second}`
}
join1<number>(1, 1)
```

```ts
function map1<T>(params: T[]) {
  return params
}
// 同
function map2<TT>(params: Array<TT>) {
  return params
}
map1<string>(['abc', '123'])
map2<number>([1, 2])
```
+  指定多个泛型
```ts
function join2<T, P>(first: T, second: P) {
  return `${first}${second}`
}
// typescript 能自动推到出泛型。这里推导出T是number，P是string
join2(1, '1')
```

+ 使用泛型作为类型注解
```ts
function hello<T>(params: T) {
  return params
}
const func: <T>(param: T) => T = hello
```

+ extends 关键字给泛型施加约束
```ts
interface Length{
  length:number
}
// 泛型T必须有length属性，值为number类型
function echoWithLength<T extends Length>(arg:T):T{
  console.log(arg.length)
  return arg
}
const str=echoWithLength('str')
const obj=echoWithLength({length:3})
const arr2 =echoWithLength([2,3,4])
```

#### 5.4.3 类中的泛型

```ts
class DataManager<T> {
  constructor(private data: T[]) {}
  getItem(index: number): T {
    return this.data[index]
  }
}
// 在使用类的时候，把泛型具象化
const data = new DataManager<string>(['1', 'a'])
console.log(data.getItem(0))
```

```ts
interface Item {
  name: string
}
// 泛型继承接口，T（数组成员）里面必须要有name属性
class DataManager<T extends Item> {
  constructor(private data: T[]) {}
  getItem(index: number): string {
    return this.data[index].name
  }
}
const data = new DataManager([
  {
    name: 'Dell',
  },
])
```

#### 5.4.4 接口中使用泛型

```ts
interface KeyPair<T,U> {
  key:T
  value:U
}
let kp1:KeyPair<number,string> = {key:1,value:'string'}
let kp2:KeyPair<string,number> = {key:'str',value:2}
let arr:number[] = [1,2,3]
let arrTwo:Array<number> = [1,2,3,4]
```



#### 5.4.5 泛型中 keyof 的使用

> 使用 keyof 遍历一个接口

```ts
interface Person {
  name: string
  age: number
  gender: string
}
class Teacher {
  constructor(private info: Person) {}
  // keyof 对Person遍历
  getInfo<T extends keyof Person>(key: T): Person[T] {
    return this.info[key]
  }
}

const teacher = new Teacher({
  name: 'dell',
  age: 18,
  gender: 'male',
})

// 限制了getInfo 方法中的参数 只能是 Person中的键
// const testName = teacher.getInfo('hello');
const testName = teacher.getInfo('age')
console.log(testName)
```

### 5.5 命名空间

- Typescript 提供了另一种封装代码的方式，**namespace** 关键字。

> Typescript 支持命名空间，但命名空间并不是封装代码的首选方式。

- 命名空间必须要有名称。命名空间可以导出函数、变量、类型、接口或其他命名空间。namespace 块中没有显式导出的代码为所在块的**私有代码**。由于命名空间可以导出命名空间，因此命名空间可以嵌套。

* 命名空间不遵守 tsconfig.json 中的 module 设置，始终编译为全局变量。

```ts
namespace Home {
  class Header {
    constructor() {
      const elem = document.createElement('div')
      elem.innerText = 'This is Header!'
      document.body.appendChild(elem)
    }
  }
  class Content {
    constructor() {
      const elem = document.createElement('div')
      elem.innerText = 'This is Content!'
      document.body.appendChild(elem)
    }
  }
  class Footer {
    constructor() {
      const elem = document.createElement('div')
      elem.innerText = 'This is Footer!'
      document.body.appendChild(elem)
    }
  }

  export class Page {
    constructor() {
      new Header()
      new Content()
      new Footer()
    }
  }
}
```

- 命名空间与模块化

> 重新组织上述代码。源代码拆离成两个代码 components.ts 和 page.ts 。 拆离后 js 文件也是分开的。
> 如果要把拆离打包后的 js 文件输出到一个文件中，在 tsconfig.json 中，outFile:"./build/page.js","module":"amd"

```ts
namespace Components {
  export class Header {
    constructor() {
      const elem = document.createElement('div')
      elem.innerText = 'This is Header!'
      document.body.appendChild(elem)
    }
  }
  export class Content {
    constructor() {
      const elem = document.createElement('div')
      elem.innerText = 'This is Content!'
      document.body.appendChild(elem)
    }
  }
  export class Footer {
    constructor() {
      const elem = document.createElement('div')
      elem.innerText = 'This is Footer!'
      document.body.appendChild(elem)
    }
  }
}
```

```ts
// 文件拆分后，尽管是不同的文件，但是仍是同一个命名空间。
/// <reference path='./components.ts'/>
namespace Home {
  export class Page {
    constructor() {
      new Components.Header()
      new Components.Content()
      new Components.Footer()
    }
  }
}
```

- 命名空间嵌套，导出接口

```ts
namespace Components {
  // 导出一个子命名空间
  export namespace SubComponents {
    export class Test {}
  }
  export interface User {
    name: string
  }
}
```

```ts
namespace Home {
  export class Page {
    user: Components.User = {
      name: 'dell',
    }
  }
}
```

### 5.6 import export

- 在 typesccript 中，应该使用 ES2015 的 import 和 export 句法，实现模块化导入导出。

* 实际开发中可以使用工具 Parcel 打包 TS 代码。

> parcel 比 webpack 简单，不需要引入 requirejs

- 安装使用 **parcel**，能解决 webpack 一些配置问题。

```bash
 npm install parcel@next -D
```

> 在 package.json 中配置"scripts"对象。表示将直接引入 html 的 ts 文件一同打包到一个 dist 目录下。

```json
"scripts":{
  "test":"parcel ./src/index.html",
}
```

## 6. 构建 TS

### 6.1 项目描述文件

- 编写 ts 项目时，首先在当前目录下初始化 npm，初始化 tscconfig

```bash
npm init -y
```

```bash
tsc --init
```

- 【一个在 ts 中使用 jquery 的项目构建】

> <<tsconfig.json>>

```json
{
  "compilerOptions": {
    "target": "es5" /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', 'ES2018', 'ES2019', 'ES2020', or 'ESNEXT'. */,
    "module": "commonjs" /* Specify module code generation: 'none', 'commonjs', 'amd', 'system', 'umd', 'es2015', 'es2020', or 'ESNext'. */,
    "outDir": "./dist" /* Redirect output structure to the directory. */,
    "rootDir": "./src" /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */,
    "strict": true /* Enable all strict type-checking options. */,
    "esModuleInterop": true /* Enables emit interoperability between CommonJS and ES Modules via creation of namespace objects for all imports. Implies 'allowSyntheticDefaultImports'. */
  }
}
```

> <<index.html>>

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.slim.min.js"></script>
    <script src="./page.ts"></script>
  </head>
  <body></body>
</html>
```

> <<page.ts>>

```ts
$(function () {
  // 即使编辑器报错，编译也可以正常进行
  $('body').html('<div>123</div>')
  new $.fn.init()
})
```

> <<jquery.d.ts>>

```ts
// 类型定义，帮助ts理解jquery语法

// 1.定义一个全局变量。
// declare var $:(params:()=>void)=>void;

// 2.直接定义一个全局函数
//  全局函数可以【重载】。同一个函数名，根据传递的参数的不同，可以定义多个全局函数。

// 这里。通过定义一个接口，把函数的返回类型定义剥离出来。
interface JqueryInstance {
  html: (html: string) => JqueryInstance
}
// 函数重载
// 2.1
declare function $(readyFunc: () => void): void
declare function $(selector: string): JqueryInstance

// --> 全局对对象进行类型定义（namespace），以及对类进行类型定义，以及命名空间的嵌套。
declare namespace $ {
  namespace fn {
    class init {}
  }
}
// 2.2
// 利用interface的特性实现函数重载的写法。直接把函数类型列入接口中。【只用于实现函数的类型】
// interface Jquery{
//   (readyFunc:()=>void):void;
//   (selector:string):JqueryInstance;
// }

// declare var $:Jquery;
```

---

---

> > 如果使用了 npm 安装 jquery

> <<jquery.d.ts>>

```ts
// ES6 模块化
//  描述jquery
declare module 'jquery' {
  interface JqueryInstance {
    html: (html: string) => JqueryInstance
  }
  // 混合类型
  function $(readyFunc: () => void): void
  function $(selector: string): JqueryInstance

  namespace $ {
    namespace fn {
      class init {}
    }
  }
  export = $
}
```

> <<page.ts>>

```ts
import $ from 'jquery'
$(function () {
  // 即使编辑器报错，编译也可以正常进行
  $('body').html('<div>123</div>')
  new $.fn.init()
})
```

---

-----以下内容待补充-----

---

## 7. 类的装饰器

- **装饰器**本身是一个函数。
- 装饰器用来修饰类。
- 装饰器通过@符号来使用。
- 装饰器接收的参数是类的构造函数。
- 类的装饰器写在类的上方，或者同行写在类的左侧，这样就能匹配对应。

- ts 中，目前装饰器的语法依然是实验性质的语法，不能直接使用，要在 tsconfig.json 中配置。

```json
/* Experimental Options */
    "experimentalDecorators": true, /* Enables experimental support for ES7 decorators. */
    "emitDecoratorMetadata": true, /* Enables experimental support for emitting type metadata for decorators. */
    /* Advanced Options */
```

```ts
function testDecorator(constructor: any) {
  console.log('decorator')
}
// 修饰：生效时间-- 类创建完成后就生效
@testDecorator
class Test {}
// const test = new Test();
```

- 注意：多个装饰器的执行顺序。

```ts
function testDecorator(constructor: any) {
  // constructor.prototype.getName = ()=>{
  //   console.log("dell")
  // }
  console.log('decorator')
}
function testDecorator2(constructor: any) {
  console.log('decorator2')
}

@testDecorator
@testDecorator2
class Test {}

const test = new Test()
// 注意装饰器的执行顺序：decorator2 decorator
```

- 注意：装饰器函数传参

```ts
function testDecorator(flag: boolean) {
  if (flag) {
    return function (constructor: any) {
      constructor.prototype.getName = () => {
        console.log('dell')
      }
    }
  } else {
    return function (constructor: any) {}
  }
}

// 修饰：生效时间-- 类创建完成后就生效
@testDecorator(true)
class Test {}

const test = new Test()

;(test as any).getName()
```

- [高级用法]：当装饰器中定义了原类中不存在的函数。

```ts
function testDecorator() {
  return function <T extends new (...args: any[]) => {}>(constructor: T) {
    return class extends constructor {
      name = 'lee'
      getName() {
        return this.name
      }
    }
  }
}

// 先修饰，再赋值
const Test = testDecorator()(
  class {
    name: string
    constructor(name: string) {
      this.name = name
    }
  }
)
// class Test{
//   name:string;
//   constructor(name:string){
//     this.name = name
//   }
// }

const test = new Test('dell')
console.log(test.getName())
```

