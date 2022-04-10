## 1，使用环境
- CommonJS模块的require语法是同步的，导致CommonJS模块规范**只适合用在服务端**。因为如果在浏览器端使用同步的方式加载一个模块，需要有网络来决定快慢，响应时间可能会很长。
- ES6模块无论在**浏览器端还是服务端都是可以使用的**，但是在服务端，还需要遵循一些特殊的规则才能使用。

## 2，输出方式不同
- CommonJS输出的是一个值的**拷贝**
- ES6模块输出的是值的**引用**

### CommonJS-值的拷贝
CommonJS输出的是值的拷贝，换句话说，一旦输出了某个值，模块内部后续的变化影响不了外部对这个值的使用 

```javascript
// lib.js
var counter = 3;
function incCounter() {
  counter++;
}
module.exports = {
  counter: counter,
  incCounter: incCounter,
};
```

然后我们在其它文件中使用这个模块：

```javascript
var mod = require('./lib');
console.log(mod.counter);  // 3
mod.incCounter();
console.log(mod.counter); // 3
```

上面的例子证明，如果我们对外输出了`conter`变量，就算后续调用模块内部的`incCounter`方法去修改它的值，它的值也不会变化

### ES6模块-值的引用
ES6模块运行机制完全不一样，JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会生成一个只读引用。等到脚本真正执行的时候，再根据这个只读引用，到被加载的那个模块里去取值。

```javascript
// lib.js
export let counter = 3;
export function incCounter() {
  counter++;
}

// main.js
import { counter, incCounter } from './lib';
console.log(counter); // 3
incCounter();
console.log(counter); // 4
```

上面证明，ES6模块`import`的变量`counter`是可变的，完全反应它在模块`lib.js`内部的变化

## 3，运行时加载和编译时加载
- CommonJS模块是**运行时加载**
- ES6模块是**编译时输出接口**

### CommonJS-运行时加载

```javascript
// CommonJS模块
let { stat, exists, readfile } = require('fs');

// 等同于
let _fs = require('fs');
let stat = _fs.stat;
let exists = _fs.exists;
let readfile = _fs.readfile;
```
上面代码的实质是整体加载fs模块（即加载fs的所有方法），生成一个对象（_fs），然后再从这个对象上面读取 3 个方法。这种加载称为“运行时加载”，因为只有运行时才能得到这个对象，导致完全没办法在编译时做“静态优化”

### ES6模块-编译时加载

```javascript
import { stat, exists, readFile } from 'fs';
```
上面代码的实质是从fs模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 ES6 可以在编译时就完成模块加载，效率要比 CommonJS 模块的加载方式高。当然，这也导致了没法引用 ES6 模块本身，因为它不是对象。

### 小结
CommonJS其实加载的是一个对象，这个对象只有在脚本运行时才会生成，而且只会生成一次。但是ES6模块不是对象，它的对外接口只是一种静态定义，在代码静态解析阶段就会生成，这样我们就可以使用各种工具对JS模块进行依赖分析，优化代码，而Webpack中的 tree shaking 和 scope hoisting 实际上就是依赖ES6模块化。

## 4，互相引用
- ES6模块中，是支持加载CommonJS模块的
- 但是，CommonJS并不支持requireES6模块


## 总结
- 因为CommonJS的require语法是同步的，导致CommonJS只适合用于服务端；而ES6模块在浏览器端和服务器端都是可用的，但是在服务端需要遵循特殊的规则
- CommonJS模块输出的是一个值的拷贝，而ES6模块输出的是值的引用
- CommonJS模块是运行时加载；ES6模块是编译时输出的接口，方便对JS模块进行静态分析
- 关于互相引用问题，ES6模块中支持加载CommonJS；而CommonJS不支持引用ES6模块