## new的作用
new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对
象的实例。

## new内部的实现流程
- 创建一个空对象`{}`
- 将构造函数的`prototype`赋值给对象的`__proto__`
- 执行构造函数，并将步骤1创建的空对象作为构造函数this的上下文
- 若函数没有返回值或者返回值不是对象，则返回创建的对象；若返回值是对象，则直接返回该对象。

demo：

```javascript
function Person (name){
    this.name = name
}
let p = new Person('jack');
console.log(`p:`, p); // { name: 'jack' }
console.log(`p.__proto__===Person.prototype:`,p.__proto__===Person.prototype);  //true
```
可以看到`new`操作符执行`Person`构造函数后，返回了一个内部创建的新对象，并且以这个对象为上线文环境执行了一遍`Person`函数，最后将其返回，同时对象`p`的原型属性指向构造函数的原型，这样也就保证了实例能够访问在构造函数原型中定义的属性和方法。

上面的demo中构造函数是没有返回值的，如果说构造函数有返回值呢，如下

```javascript
function Person (name){
    this.name = name;
    return {age: 18}
}
let p = new Person('jack');
console.log(`p:`, p); // { age: 18 }
```
如果构造函数最后返回了一个对象，就会直接将其返回，而不是内部创建的新对象。

## 更多栗子

```javascript
function Person(name) {
    this.name = name;
}
function Person2(name) {
    this.name = name;
    return this.name;
}
function Person3(name) {
    this.name = name;
    return new Array();
}
function Person4(name) {
    this.name = name;
    return new String(name);
}
function Person5(name) {
    this.name = name;
    return function() {};
}


var person = new Person('John');  // {name: 'John'}
var person2 = new Person2('John');  // {name: 'John'}
var person3 = new Person3('John');  // []
var person4 = new Person4('John');  // 'John'
var person5 = new Person5('John');  // function() {}
```


## 伪代码实现new

```javascript
new Person("John") = {
    let obj = {};
	obj.__proto__ = Person.prototype; // 此时便建立了obj对象的原型链： obj->Person.prototype->Object.prototype->null
	let result = Person.call(obj,"John"); // 相当于obj.Person("John")
	return typeof result === 'object' ? result : obj; // 如果无返回值或者返回一个非对象值，则将obj返回作为新对象
}
```