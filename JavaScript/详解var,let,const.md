ES6中新增了块级作用域、let和const
## 1.var命令
**函数作用域**

在使用var声明变量时，变量会被自动添加到最近的上下文。如果变量**未经声明就被初始化**了，那么它就会自动被添加到**全局上下文**

```javascript
function add(num1,num2){
	var sum=num1+num2
	return sum
}

let result=add(10,20)
console.log(sum)	// 报错:sum在这里不是有效变量
```
这里，函数add定义了一个局部变量sum，这个值作为函数的值被返回，但变量sum在函数外是访问不到的。

```javascript
function add(num1,num2){
	sum=num1+num2
	return sum
}

let result=add(10,20)
console.log(sum)	// 30
```
若省略了关键字var，在调用add之后，sum被添加到了全局上下文。

**注意：未经声明而初始化变量是JS中一个非常常见的错误，会导致很多问题。**

------------

## 2.let命令
## 基本用法

ES6 新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，**只在let命令所在的代码块内有效。**（即块级作用域）

```javascript
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```
上面代码在代码块之中，分别用let和var声明了两个变量。然后在代码块之外调用这两个变量，结果let声明的变量报错，var声明的变量返回了正确的值。这表明，let声明的变量只在它所在的代码块有效。

下面的代码如果使用var，最后输出的是10。

```javascript
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```
上面代码中，变量i是var命令声明的，在全局范围内都有效，所以**全局只有一个变量i**。每一次循环，变量i的值都会发生改变，而循环内被赋给数组a的函数内部的console.log(i)，里面的i指向的就是全局的i。也就是说，所有数组a的成员里面的i，**指向的都是同一个i**，导致运行时输出的是最后一轮的i的值，也就是 10。

如果使用let，声明的变量仅在块级作用域内有效，最后输出的是 6。

```javascript
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```
上面代码中，变量i是let声明的，当前的i**只在本轮循环有效**，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算。

## 不存在变量提升
**var命令会发生“变量提升”现象，即变量可以在声明之前使用，值为undefined**。这种现象多多少少是有些奇怪的，按照一般的逻辑，变量应该在声明语句之后才可以使用。

为了纠正这种现象，let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

```javascript
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```
上面代码中，变量foo用var命令声明，会发生变量提升，即脚本开始运行时，变量foo已经存在了，但是没有值，所以会输出undefined。变量bar用let命令声明，不会发生变量提升。这表示在声明它之前，变量bar是不存在的，这时如果用到它，就会抛出一个错误。

## 暂时性死区
只要块级作用域内存在let命令，它所声明的变量就“绑定”（binding）这个区域，不再受外部的影响。

```javascript
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```
上面代码中，存在全局变量tmp，但是块级作用域内let又声明了一个局部变量tmp，导致后者绑定这个块级作用域，所以在let声明变量前，对tmp赋值会报错。

ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

总之，**在代码块内，使用let命令声明变量之前，该变量都是不可用的**。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

## 不允许重复声明
let不允许在相同作用域内，重复声明同一个变量。

```javascript
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}
```


## 为什么需要块级作用域？

**第一种场景，内层变量可能会覆盖外层变量。**

```javascript
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = 'hello world';
  }
}

f(); // undefined
```
上面代码的原意是，if代码块的外部使用外层的tmp变量，内部使用内层的tmp变量。但是，函数f执行后，输出结果为undefined，原因在于变量提升，导致**内层的tmp变量覆盖了外层的tmp变量**。

**第二种场景，用来计数的循环变量泄露为全局变量。**

```javascript
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
```
上面代码中，变量i只用来控制循环，但是循环结束后，它**并没有消失，泄露成了全局变量**。


## ES6的块级作用域

```javascript
function f1() {
  let n = 5;
  if (true) {
    let n = 10;
  }
  console.log(n); // 5
}
```
上面的函数有两个代码块，都声明了变量n，运行后输出 5。这表示外层代码块不受内层代码块的影响。如果两次都使用var定义变量n，最后输出的值才是 10。


## 3.const命令
const声明一个**只读的常量**。一旦声明，常量的值就不能改变。

```javascript
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```

const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。

**const的作用域与let命令相同**：只在声明所在的块级作用域内有效。

**const命令声明的常量也是不提升，同样存在暂时性死区**，只能在声明的位置后面使用。

const声明的常量，也与let一样**不可重复声明**。

## 本质
const实际上保证的，并不是变量的值不得改动，而是变量**指向的那个内存地址所保存的数据**不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

```javascript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```
上面代码中，常量foo储存的是一个地址，这个地址指向一个对象。不可变的只是这个地址，即**不能把foo指向另一个地址**，**但对象本身是可变的**，所以依然可以为其**添加新属性**。

部分摘自 [阮一峰博客](https://es6.ruanyifeng.com/#docs/let)