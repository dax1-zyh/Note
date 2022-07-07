## 1. for...in

利用`for...in`遍历对象，如果对象存在属性则返回“非空”，否则返回“空”
```javascript
        function fn(obj) {
            for (let key in obj) {
                return '非空'
            }
            return '空'
        }
```


## 2. JSON.Stringify()
利用`JSON.stringify()`将对象序列化，若序列化后的结果为`{}`，则对象是空对象

```javascript
        function fn2(obj) {
            let res = JSON.stringify(obj)
            return res === '{}' ? '空' : '非空'
        }
```

## 3. Object.keys()

利用`Object.keys()`返回一个由给定对象的自身**可枚举属性**组成的数组，若返回一个空数组，则给定对象为空对象。
```javascript
        function fn3(obj) {
            return Object.keys(obj).length === 0 ? '空' : '非空'
        }
```