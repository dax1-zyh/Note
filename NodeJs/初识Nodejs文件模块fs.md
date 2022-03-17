## 什么是fs模块
Node.js内置的fs模块为文件系统模块，负责对文件的操作。
fs模块同时提供了同步和异步方法，在名称上的区别为同步方法后有`Sync`

## 引入模块
` const fs = require('fs') `

## 读取文件

### 同步读取方法 readFileSync
readFileSync有两个参数

 - 第一个参数为读取文件的路径
 - 第二个参数是编码方式

返回值：文件的内容。

```javascript
const fs = require("fs");

let buf = fs.readFileSync("1.txt");
let data = fs.readFileSync("1.txt", "utf8");

console.log(buf); // <Buffer 48 65 6c 6c 6f>
console.log(data); // Hello
```
如果同步读取文件时发生错误，需要用`try...catch`捕获错误

```javascript
try {
    const data = fs.readFileSync('sample.txt', 'utf-8');
    console.log(data);
} catch (err) {
    // 出错了
}
```

### 异步读取方法 readFile
异步方法的参数和同步方法前两个参数相同，最后一个参数为回调函数，该回调含有两个参数`err（错误）和data（文件内容）`，该方法没有返回值。

```javascript
const fs = require("fs");

fs.readFile("1.txt", "utf8", (err, data) => {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
```

### Buffer与String的相互转换
当读取一个二进制文件时，获取到的内容是一个Buffer，可以将一个Buffer对象转换成String

```javascript
// Buffer -> String
var text = data.toString('utf-8');
console.log(text);
```

或者把一个String转换成Buffer

```javascript
// String -> Buffer
var buf = Buffer.from(text, 'utf-8');
console.log(buf);
```

## 写入文件
### 同步写入方法 writeFileSync
writeFileSync有三个参数

 - 第一个参数为要写入的文件路径
 - 第二个参数为写入的数据，类型为String或Buffer
 - 第三个参数为编码方式

没有返回值

```javascript
const fs = require("fs");

fs.writeFileSync("2.txt", "Hello world");
let data = fs.readFileSync("2.txt", "utf8");

console.log(data); // Hello world
```

### 异步写入方法 writeFile
writeFile与writeFileSync的前三个参数相同，最后一个参数为回调函数，回调函数只关心成功与否，因此只有一个参数`err`

```javascript
const fs = require("fs");

fs.writeFile("2.txt", "Hello world", err => {
    if (!err) {
        fs.readFile("2.txt", "utf8", (err, data) => {
            console.log(data); // Hello world
        });
    }
});
```

**注意：写入方法在目标路径不存在时，会创建文件，但不会创建目录。若路径中包含目录，务必保证目录是存在的。**

## 追加写入文件
### 同步追加写入方法 appendFileSync
appendFileSync有三个参数
- 第一个参数为写入的文件路径
- 第二个参数为写入的数据，类型为String或Buffer
- 第三个参数为编码方式

可以看出其参数与写入文件方法writeFileSync完全一样，只是一个为覆盖内容，一个为追加内容。

```javascript
const fs = require("fs");

fs.appendFileSync("3.txt", " world");
let data = fs.readFileSync("3.txt", "utf8");

console.log(data); // Hello world
```

### 异步追加写入方法 appendFile
异步方法与同步方法前三个参数相同，最后一个参数为回调函数，同样只关心成功与否，回调函数只有一个参数`err`

```javascript
const fs = require("fs");

fs.appendFile("3.txt", " world", err => {
    if (!err) {
        fs.readFile("3.txt", "utf8", (err, data) => {
            console.log(data); // Hello world
        });
    }
});
```

## 拷贝写入文件
### 同步拷贝写入方法 copyFileSync
copyFileSync方法有两个参数

 - 第一个参数为被拷贝的源文件路径
 - 第二个参数为拷贝到的目标文件路径，若目标文件不存在，则会创建文件


```javascript
 // 将3.txt的内容拷贝到4.txt
 
const fs = require("fs");

fs.copyFileSync("3.txt", "4.txt");
let data = fs.readFileSync("4.txt", "utf8");

console.log(data); // Hello world
```

### 异步拷贝写入方法 copyFile
异步方法与同步方法前两个参数相同，最后一个参数为回调函数。

```javascript
const fs = require("fs");

fs.copyFile("3.txt", "4.txt", () => {
    fs.readFile("4.txt", "utf8", (err, data) => {
        console.log(data); // Hello world
    });
});
```

### 用读取方法和写入方法模拟拷贝写入方法
#### 模拟同步拷贝

```javascript
const fs = require("fs");

function copy(src, dest) {
    let data = fs.readFileSync(src);
    fs.writeFileSync(dest, data);
}

// 拷贝
copy("3.txt", "4.txt");

let data = fs.readFileSync("4.txt", "utf8");
console.log(data); // Hello world
```

#### 模拟异步拷贝

```javascript
const fs = require("fs");

function copy(src, dest, cb) {
    fs.readFile(src, (err, data) => {
        // 没错误就正常写入
        if (!err) fs.writeFile(dest, data, cb);
    });
}

// 拷贝
copy("3.txt", "4.txt", () => {
    fs.readFile("4.txt", "utf8", (err, data) => {
        console.log(data); // Hello world
    });
});
```

## 获取文件目录的stats对象
文件目录的stats对象存储着关于文件的重要信息，例如文件大小、创建时间等，可以使用`fs.stat()`方法，他返回一个`stat`对象

### 同步获取stats对象方法 statSync
statSync参数为目标文件的路径
返回值：stats对象
```javascript
const fs = require("fs");

let stat = fs.statSync("a/b/c.txt");
console.log(stat); 
```

### 异步获取stats对象方法 stat
异步方法的第一个参数为目标路径，第二个参数为回调函数，含有`err和stats`两个参数

```javascript
const fs = require("fs");

fs.stat("a/b/c.txt", (err, stats) => {
    console.log(stats); 
});
```

## 异步方法 vs 同步方法
在fs模块中，提供同步方法是为了方便使用。那我们到底是应该用异步方法还是同步方法呢？

由于Node环境执行的JavaScript代码是服务器端代码，所以，**绝大部分需要在服务器运行期反复执行业务逻辑的代码，必须使用异步代码**，否则，同步代码在执行时期，服务器将停止响应，因为JavaScript只有一个执行线程。

服务器启动时如果需要读取配置文件，或者结束时需要写入到状态文件时，可以使用同步代码，因为这些代码只在启动和结束时执行一次，不影响服务器正常运行时的异步执行。

## 参考文档
[廖雪峰博客](https://www.liaoxuefeng.com/wiki/1022910821149312/1023025763380448)
[Node中fs模块 API详解](https://juejin.cn/post/6844903677782654983#heading-29)