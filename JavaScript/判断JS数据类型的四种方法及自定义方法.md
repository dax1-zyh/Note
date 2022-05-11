## 前言
在ECMAScript规范中，一共有8种数据类型，分为基本类型和引用类型（复杂类型），

 - 基本类型：String,Number,Boolean,Symbol,Bigint,Null,Undefined
 - 引用类型：Object

## 方法一，typeof()
使用typeof直接返回数据类型字段，但是无法判断Null，数组，对象

```javascript
console.log(typeof bool); //boolean
console.log(typeof num);//number
console.log(typeof str);//string
console.log(typeof und);//undefined
console.log(typeof nul);//object
console.log(typeof arr);//object
console.log(typeof obj);//object
console.log(typeof fun);//function
```
可以看出，对于 Null，数组，对象，typeof检测结果均为object。
对于函数类型，返回Function


## 方法二，instance of()
instance of 用来判断A是否为B的实例 ，而不能判断一个对象实例具体属于哪种类型。
instance of 检测的是**原型**

```javascript
console.log(bool instanceof Boolean);// false
console.log(num instanceof Number);// false
console.log(str instanceof String);// false
console.log(und instanceof Object);// false
console.log(arr instanceof Array);// true
console.log(nul instanceof Object);// false
console.log(obj instanceof Object);// true
console.log(fun instanceof Function);// true

var bool2 = new Boolean()
console.log(bool2 instanceof Boolean);// true

var num2 = new Number()
console.log(num2 instanceof Number);// true

var str2 = new String()
console.log(str2 instanceof String);//  true

function Person(){}
var per = new Person()
console.log(per instanceof Person);// true

function Student(){}
Student.prototype = new Person()
var haoxl = new Student()
console.log(haoxl instanceof Student);// true
console.log(haoxl instanceof Person);// true
```

instance of 不能区别Null和Undefined

## 方法三，constructor
undefined和null没有contructor属性

```javascript
console.log(bool.constructor === Boolean);// true
console.log(num.constructor === Number);// true
console.log(str.constructor === String);// true
console.log(arr.constructor === Array);// true
console.log(obj.constructor === Object);// true
console.log(fun.constructor === Function);// true

console.log(haoxl.constructor === Student);// false
console.log(haoxl.constructor === Person);// true
```

## 方法四，toString()
toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]] 。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。

对于 Object 对象，直接调用 toString()  就能返回 [object Object] 。而对于其他对象，则需要通过 call / apply 来调用才能返回正确的类型信息。

call()方法可以改变this的指向，那么把Object.prototype.toString()方法指向不同的数据类型上面，返回不同的结果

```javascript
Object.prototype.toString.call(1)
"[object Number]"

Object.prototype.toString.call(NaN);
"[object Number]"

Object.prototype.toString.call("1");
"[object String]"

Object.prototype.toString.call(true)
"[object Boolean]"

Object.prototype.toString.call(null)
"[object Null]"

Object.prototype.toString.call(undefined)
"[object Undefined]"

Object.prototype.toString.call(function a() {});
"[object Function]"

Object.prototype.toString.call([]);
"[object Array]"

Object.prototype.toString.call({});
"[object Object]"
```

## 自定义一个完美的判断数据类型的方法
参考文档： [javascript 判断数据类型的几种方法](https://segmentfault.com/a/1190000018160547)

```javascript
function _typeof(obj){
  var s = Object.prototype.toString.call(obj);
  return s.match(/\[object (.*?)\]/)[1].toLowerCase();
};


_typeof([12,3,343]);
"array"

_typeof({name: 'zxc', age: 18});
"object"

_typeof(1);
"number"

_typeof("1");
"string"

 _typeof(null);
"null"

_typeof(undefined);
"undefined"

_typeof(NaN);
"number"

_typeof(Date);
"function"

_typeof(new Date());
"date"

_typeof(new RegExp());
"regexp"
```