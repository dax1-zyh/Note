两个模拟数组

```js
let arr = [3,4,5,5,6,6,7,8];
let arr2 = [1,2,3,3,4,4,5];
```



## 去重

```js
let result = [...new Set(arr)];

conlose.log(result);	// [3,4,5,6,7,8]
```



## 交集

```js
let result = [...new Set(arr)].filter(item=>{
	let result2 = new Set(arr2);	//这里需要是一个Set对象才能用下一行的has方法
	if(result2.has(item)){
		return true;
	}else{
		return false;
	}
})
console.log(result);	//[3,4,5]
```



↑ 第二行可简化为

```js
let result = [...new Set(arr)].filter(item => new Set(arr2).has(item)) 
```



## 并集

```js
let result = [...new Set([...arr,...arr2])]		//先合并两个数组再去重
console.log(result);	//[3, 4, 5, 6, 7, 8, 1, 2]
```



## 差集

差集就相当于是交集的逆运算

```js
let result = [...new Set(arr)].filter(item => !new Set(arr2).has(item));
console.log(result);	//[6,7,8]
```

