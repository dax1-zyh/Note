## 什么是模块化
模块化指解决一个复杂问题是，自顶向下逐层**把系统划分为若干模块**的过程。对于整个系统来说，模块是可组合、分解和更换的单元

## 模块化的优势
- 提高了代码的**复用性**
- 提高了代码的**可维护性**
-  可以实现**按需加载**

## Node.js中模块的分类
- 内置模块（由Node.js官方提供的，例如fs、path、http等）
- 自定义模块（用户创建的每个js文件都是自定义模块）
- 第三方模块（由第三方开发出来的模块，使用前需要先下载）

## 加载模块

```javascript
// 加载内置的fs模块
const fs = require('fs')

// 加载用户的自定义模块
const custom = require('./custom.js')

// 加载第三方模块,需要先下载
const moment = require('moment')
```

**注意：**
- 使用`require()`方法来加载其他模块时，会执行被加载模块中的代码。
- 引入自定义模块时，不能只写模块名，不要忘记写相对路径

## 模块作用域
与函数作用域类似，在自定义模块中定义的变量、方法等，只能在当前模块中被访问，这种模块级别的访问限制，叫做模块作用域。

**作用**：防止全局变量污染

## 向外暴露模块作用域中的成员
### module对象
在每个自定义模块中都有一个`module`对象，它存储了和当前模块有关的信息。

### module.exports对象
在自定义模块中，可以使用`module.exports`对象，将模块内的成员共享出去，供外界使用。

外界用`require()`方法导入自定义模块时，**得到的就是`module.exports`所指的对象**。

```javascript
// index.js

const age = 20
module.exports.age = 18
module.exports.name = 'frank'
```

```javascript
// index2.js

const m = require('./index.js')
console.log(m)
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/227f0f526f854a2184279bb37536e794.png)
可以看到，在`index.js`中定义的age没有向外暴露，外界只获取到了`module.exports`对象中的属性。

### 导入模块的结果，永远以`module.exports`指向的对象为准

```javascript
// index.js

module.exports.name = 'frank'
module.exports.age = '18'

// 让module.exports指向一个全新的对象
module.exports = {
    name:'mike',
    age:20
}
```

```javascript
// index2.js

const m = require('./index.js')
console.log(m)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/191b67334f8d444e811cc398ca69a111.png)
### exports对象
由于`module.exports`写起来比较复杂，Node提供了`exports`对象，默认情况下，**`exports`和`module.exports`指向同一个对象**，但最终暴露的结果，还是以`module.exports`指向的对象为准。

### exports和module.exports的使用误区
![在这里插入图片描述](https://img-blog.csdnimg.cn/e73ccb1a6f354e86a7fcade4df2a2f34.png)

上图，是四种向外暴露的结果，结合“**导入模块的结果，永远以`module.exports`指向的对象为准**”，仔细分析。

（简单地理解：给`exports`直接赋值是无效的，因为赋值后`module.exports`仍然是空对象，这里是一个陷阱。）

**结论：** 为了防止混乱，不要在同一个模块中同时使用`exports`和`module.exports`。

## 模块化规范CommonJS
Node.js遵循了CommonJS模块化规范，他规定了模块的特性和各模块之间如何相互依赖。

CommomJS规定：
- 每个模块内部，`module`变量代表当前模块
- `module`变量是一个对象，它的`exports`属性是对外的接口
- `require()`方法用于加载模块
- 加载某个模块，其实是加载该模块的`modele.exports`属性