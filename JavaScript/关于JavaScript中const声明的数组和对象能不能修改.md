我们知道const的特点是 声明一个不可修改的常量，且必须在声明的同时初始化值。

但是const还有一个特点就是可以更改对象的属性

Array和Object都是引用类型，当用const声明数组和对象时，const声明的常量保存的仅仅是目标的指针，这就意味着只要保证数组和对象的指针不发生改变，修改其中的值是被允许的。

例如

```javascript
const arr = [1,2,3,4,5]
arr[0] = 2			// 合法
arr.push(6)			// 合法
console.log(arr)	//[2,2,3,4,5,6]
arr = [1,2,3]		// 错误
```
通过数组的索引值或者数组方法对数组进行的修改是被允许的，但是不允许对常量直接赋值。

对象也是同理

```javascript
const obj = {name:'frank', age=18}
obj.age = 19 		// 合法
obj.weight = 100	// 合法
console.log(obj)	// {name:'frank',age=19,weight=100}
obj = {name:'lisa'}	// 错误
```
修改对象的属性是被允许的，但是不允许直接给对象赋值