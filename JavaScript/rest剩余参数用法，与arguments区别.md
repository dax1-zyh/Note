## rest 剩余参数

rest语法允许我们将一个不定数量的参数表示为一个数组

```js
function sum(...theArgs) {
  return theArgs.reduce((previous, current) => {
    return previous + current;
  });
}

console.log(sum(1, 2, 3));
// expected output: 6

console.log(sum(1, 2, 3, 4));
// expected output: 10

```



若函数的最后一个形参以 `...` 为前缀，则它将成为一个由剩余参数组成的**真数组**，rest参数必须放到参数最后。



## rest 与 arguments对象的区别

- rest参数只包含那些**没有对应形参的实参**，而arguments对象包含了传给函数的**所有实参**
- arguments对象不是一个真正的数组，而rest是真正的Array实例（**真数组**）
- arguments对象还有一些附加属性（如callee属性）



## 解构剩余参数

与解构赋值相结合

```js
function f(...[a, b, c]) {
  return a + b + c;
}

f(1)          // NaN (b and c are undefined)
f(1, 2, 3)    // 6
f(1, 2, 3, 4) // 6 (the fourth parameter is not destructured)
```

