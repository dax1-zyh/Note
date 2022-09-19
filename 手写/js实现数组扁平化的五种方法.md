## 1.  Array.prototype.flat
`flat()` 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。

```javascript
let arr = [1,2,[3,4],5,6]
arr = arr.flat(Infinity)
console.log(arr) // [1,2,3,4,5,6]
```

## 2. 普通递归

```javascript
function flatten2(arr){
    let res = []
    for(let i = 0; i < arr.length; i++) {
        const item = arr[i]
        if(Array.isArray(item)) {
            res = res.concat(flatten(item))
        } else {
            res.push(item)
        }
    }
    return res
}

```

## 3. reduce实现递归

```javascript
function flatten3(arr){
    return arr.reduce((prev,curr) => {
        return prev.concat(Array.isArray(curr) ? flatten3(curr) : curr)
    },[])
}
```

## 4. 扩展运算符
扩展运算符是ES6的新特性之一，用它操作数组可以直接展开数组的第一层，利用这个特性，我们可以不使用递归来实现数组的展平，这是因为每一次递归都是对当前层次数组的一次展开，而扩展操作符就是干这工作的。

```javascript
// 4.扩展运算符
function flatten4(arr){
    while(arr.some(i => Array.isArray(i))) {
        // ...arr => 1 2 [3 , 4] 5 6
        arr = [].concat(...arr)
    }
    return arr
}
```

## 5. 正则表达式

```javascript
function flatten5(arr) {
    let str = JSON.stringify(arr);
    str = str.replace(/(\[|\])/g, '');
    // 拼接最外层，变成JSON能解析的格式
    str = '[' + str + ']';
    return JSON.parse(str);
}
```