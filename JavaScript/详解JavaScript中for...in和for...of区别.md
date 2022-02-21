## 前言
简单地说，for...in一般用来遍历对象的**键名(key)**，for...of一般用来遍历数组的**键值(value)**。

但这只是浅显的说法，需要更加深入的理解才能知道他们的区别。

## for...in
for...in循环遍历的是**可枚举属性（包括原型链上的可枚举属性）**。

```javascript
var obj = {a:1, b:2, c:3};
    
for (let key in obj) {
  console.log(key);
}

// a
// b
// c
```
使用for...in 也可以遍历数组，但是会存在以下问题：

 - index索引为字符串型数字，不能直接进行几何运算
 - 遍历顺序有可能不是按照实际数组的内部顺序
 - 使用for in会遍历数组所有的可枚举属性，包括原型。例如上栗的原型方法method和name属性
   

## for...of
for...of是ES6新引入的特性，for...of的出现是为了方便迭代器的使用。

for...of语句在**可迭代对象**（包括Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，对每个不同属性的属性值,调用一个自定义的有执行语句的迭代挂钩。

也就是说，for...of只可以循环可迭代对象的可迭代属性，不可迭代属性在循环中被忽略了。

for...of**不可以遍历普通对象**，想要遍历对象的属性，可以用for...in循环, 或内建的Object.keys()方法

```javascript
const array1 = ['a', 'b', 'c'];

for (const val of array1) {
  console.log(val);
}

// a
// b
// c
```

## for...in和for...of对比
```javascript
for (var key in arr){
    console.log(arr[key]);
}

for (var value of arr){
    console.log(value);
}
```

这两者效果是一样的

--------

```js
Object.prototype.objCustom = function () {}; 
Array.prototype.arrCustom = function () {};

let iterable = [3, 5, 7];
iterable.foo = "hello";

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}
```
可以看到，对于array的不可迭代元属性objCustom、arrCustom和实例属性foo，在循环中都被忽略。
这是for...in迭代KEY，for of迭代value之外最大的区别


## 小结

 - for...in以任意顺序迭代对象的**可枚举属性**，一般用于遍历对象的key
 - for...of语句遍历**可迭代对象**定义要迭代的数据，一般用于遍历数组的value 