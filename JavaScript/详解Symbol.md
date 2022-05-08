## Symbol是什么

Symbol是ES6新推出的一种基本类型，它表示**独一无二的值**

Symbol最大的用途是用来定义对象的唯一属性名，例如要给一个已有属性的对象添加一个新的属性，新的属性可能和旧的属性名称冲突，这个时候采用Symbol是最好的。



## 定义Symbol

它可以选择接受一个字符串作为参数或者没有参数，但是相同参数的两个Symbol值也是不相等的。

```js
//不传参数
const s1 = Symbol();
const s2 = Symbol();
console.log(s1 === s2); // false

// 传入参数
const s3 = Symbol('debug');
const s4 = Symbol('debug');
console.log(s3 === s4); // false

```



## 用typeof判断数据类型

```js
console.log(type of s1)	//symbol
```



## Symbol不是一个完整的构造函数，不能通过 `new Symbol()`来创建

```js
const s1 = new Symbol();
// Uncaught TypeError: Symbol is not a constructor
```



## Symbol值不能与其他类型的值进行运算

也不能多个Symbol值进行运算



## Symbol值不是对象，不能添加属性



## Symbol值可以显示地转为字符串，也可以转为布尔值，但是不能转为数值

```js
var symbol = Symbol("Alice");  

//可以通过toString方法转化为字符串
console.log(symbol.toString()); // 输出：Symbol(Alice)
  
//可以通过Boolean方法转化为
console.log(Boolean(symbol)); // 输出：true  
if (symbol)  
    console.log("YES"); // 输出：Yes 
    
//就算是一个空的Symbol返回的也是true
var symbol1 = Symbol();  
console.log(Boolean(symbol1)); // 输出：true  

//不能转化为数字类型的，会报错
console.log(Number(symbol)); // 报错：TypeError  
```



## Symbol方法

Symbol有两个方法

- symbol.for()
- symbol.keyFor()



### Symbom.for()

用于将描述相同的Symbol变量指向同一个Symbol值

```js
let a1 = Symbol.for('a');
let a2 = Symbol.for('a');
a1 === a2  // true
typeof a1  // "symbol"
typeof a2  // "symbol"

let a3= Symbol("a");
a1 === a3      // false
```

它与`Symbol()`的区别在于，`Symbol()`定义的值每次都是新建，即使描述相同，也是不相等的值

而 `Symbol.for()`定义的值会先检查给定的描述是否已经存在，若以不存在才会新建一个值，否则描述相同则它们就是相同的值。



### Symbol.keyFor()

用来检测字符串参数作为名称的Symbol值是否已被创建，返回一个已创建的Symbol类型值的`key`

```js
let a1 = Symbol.for("a");
Symbol.keyFor(a1);    // "a"

let a2 = Symbol("a");
Symbol.keyFor(a2);    // undefined
```



## Symbol属性

### description

用来返回Symbol数据的描述

```js
// Symbol()定义的数据
let a = Symbol("acc");
a.description  // "acc"
Symbol.keyFor(a);  // undefined

// Symbol.for()定义的数据
let a1 = Symbol.for("acc");
a1.description  // "acc"
Symbol.keyFor(a1);  // "acc"

// 未指定描述的数据
let a2 = Symbol();
a2.description  // undefined
```



## 使用场景

为对象添加属性，防止新属性名与原有的属性名冲突，

```js
var name = Symbol();  
var obj1 = {  
  //Symbol类型的属性
    [name]: "Alice"  
};  
var obj2 = {  
  //字符串类型的属性
    name: "Bruce"  
};  
console.log(obj1.name); // 输出：undefined  
console.log(obj1[name]); // 输出：Alice  
console.log(obj2.name); // 输出：Bruce  
console.log(obj2[name]); // 输出：undefined
```

当使用Symbol作为对象属性的时候，需要注意

- 属性名需要用中括号的形式添加
- 需要通过方括号的形式访问Symbol属性
- 迭代属性的时候，某些情况不能得到该Symbol属性，如 `for...in`、`for...of`





