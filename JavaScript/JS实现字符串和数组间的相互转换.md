## 字符串转数组
### 1.String.split()
String对象中的split()方法

上述方法的功能是：将一个字符串切割成若干段，返回一个数组。

如：str.split(',')，用指定的参数将字符串切成若干段。

```javascript
const str = 'abcd,efg,hijk'
const str2 = str.split(',')
console.log(str2)	//['abcd','efg','hijk']
```

### 2. 扩展运算符(...)
ES6引入的扩展运算符

```javascript
var str = 'hello';

// ES5 处理方式
var es5Arr = str.split('');
console.log(es5Arr) // ["h", "e", "l", "l", "o"]

// ES6 处理方式
var es6Arr = [...str];
console.log(es6Arr) // ["h", "e", "l", "l", "o"]
```


### 3. Array.from(String)

Array.from 可以将类数组对象转换成数组。

```javascript
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]

console.log(Array.from([1, 2, 3], x => x + x));
// expected output: Array [2, 4, 6]
```


## 数组转字符串
### 1.Array.join()
join() 方法将数组作为字符串返回。

元素将由指定的分隔符分隔。默认分隔符是逗号 (,)

**注释**：join() 方法不会改变原始数组。

```javascript
const fruits = ["Banana", "Orange", "Apple", "Mango"];
const energy = fruits.join();
console.log(energy) //"Banana,Orange,Apple,Mango"
```



### 2.Array.toString()

toString() 返回一个字符串，表示指定的数组及其元素。

```javascript
const array1 = [1, 2, 'a', '1a'];

console.log(array1.toString());
// expected output: "1,2,a,1a"
```



## 面试题

**题目**
利用var s1=prompt("请输入任意的字符串","")可以获取用户输入的字符串，试编程将用户输入的字符串“反转”，并且将字符串输出。

**思路**
想了一下，字符串对象的方法中并没有实现反转的，但是数组中有，于是考虑了字符串和数组的相互转换问题。


 - 将字符串转换为数组
 - 将数组反转
 - 将反转后的数组转换为字符串输出

**实现代码**

```javascript
    //接受字符串
    let s1=prompt("请输入任意的字符串","");
    //字符串转换为数组
    let arr=s1.split("");
    //利用数组对象的reverse()方法实现反转
    arr.reverse();
    //利用数组的join()方法转换为字符串
    let str=arr.join("");
    //得到输出结果str
```