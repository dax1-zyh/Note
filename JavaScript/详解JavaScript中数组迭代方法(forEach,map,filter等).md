## forEach
forEach方法对数组中的每个元素执行一次提供的函数

### 使用方法
```javascript
array.forEach(callback(currentVal, index, array) {
  // do something
}, thisArg)
```

 - callback
 - currentVal:数组中正在处理的当前元素
 - index:数组中正在处理的当前元素的索引
 - array:forEach方法正在操作的数组
 - thisArg: 当执行回调参数callback时，用作this的值

### 注意点

 - 没有返回值，为undefined
 - 不改变原数组
 - 不能中止或跳出forEach循环
 - 使用箭头函数，thisArg参数会被忽略


```javascript
var arr1 = [1, 2, 3]
var arr2 = [7, 8, 9]

arr1.forEach((v, i, arr) => {
  console.log(this)
})
// window
// window
// window

arr1.forEach((v, i, arr) => {
  console.log(this)
}, arr2)
// window
// window
// window
```

 - forEach不会在迭代之前创建数组的副本，如果数组在迭代时被修改了，则其他元素会被跳过

```javascript
var words = ["one", "two", "three", "four"];
words.forEach(function(word) {
  console.log(word);
  if (word === "two") {
    words.shift();
  }
});
// one
// two
// four
```

 当到达包含值 "two" 的项时，整个数组的第一个项被移除了，这导致所有剩下的项上移一个位置。因为元素 "four" 现在在数组更前的位置，"three" 会被跳过。 forEach() 不会在迭代之前创建数组的副
本。

-----
## map
map方法创建一个数组，其结果是该数组中的每个元素都调用一个提供的函数后返回的结果。

### 使用方法

```javascript
let new_array = arr.map(function(v, i, arr) {
  // Return element for new_array 
}[, thisArg])
```

参数与forEach相同


### 注意点

 - callback函数必须return
 - 有返回值，由原数组中每个元素执行回调的结果（包括undefined）组成的新数组
 - 不改变原数组
 - callback函数只在有值的索引上被调用，那些从未被赋值或使用delete删除的索引不会被调用。
 - 数组元素范围在callback第一次调用之前就已确定（与forEach一样）


### 陷阱
通常情况下，map 方法中的 callback 函数只需要接受一个参数，就是正在被遍历的数组元素本身。但这并不意味着 map 只给 callback 传了一个参数。这个思维惯性可能会让我们犯一个很容易犯的错误

```javascript
// 下面的语句返回什么呢：
["1", "2", "3"].map(parseInt);
// 你可能觉得会是 [1, 2, 3]
// 但实际的结果是 [1, NaN, NaN]
```
通常使用 parseInt 时，只需要传递一个参数.但实际上，parseInt 可以有两个参数，第二个参数是进制数（2-36之间的整数）.map 方法在调用 callback 函数时，会给它传递三个参数：当前正在遍历的元素，元素索引，原数组本身.第三个参数 parseInt 会忽视，但第二个参数不会，也就是说：parseInt 把传过来的索引值当成进制数来使用.实际上他分别执行的是`parseInt(1,0),parseInt(2,1),parseInt(3,2)` ，因此后两次结果为NaN.

**这里用箭头函数写更为恰当**
```javascript
['1', '2', '3'].map( str => parseInt(str))
```

------------

## filter
filter() 方法创建一个新数组, 其包含通过所提供函数测试的所有元素。 

```javascript
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]

```

### 使用方法

```javascript
var new_array = arr.filter(callback[, thisArg])
```

### 注意点

 - 不改变原数组
 - 有返回值，为其含通过所提供函数测试的所有元素的新数组
 - filter 为数组中的每个元素调用一次callback函数，并利用所有使得callback返回true或等价于true的值的元素创建一个新数组。
 - 数组元素范围在callback第一次调用之前就已确定（同上）


-----

## every
检查数组中的项是否满足某个条件，传入的函数对每一项都返回true,则返回true

```javascript
var nums = [1,2,3,4,5,4,3,2,1]
nums.every((item, index, arr)=> item >2 ) //false
```
-------

## some
检查数组中的项是否满足某个条件，只要传入的函数对数组中某一项返回true,则返回true
```javascript
var nums = [1,2,3,4,5,4,3,2,1]
nums.some((item, index, arr)=> item >2 ) //true
```


## 参考文档
[JavaScript 中 forEach、map、filter 详细](https://juejin.cn/post/6844903807176933384)