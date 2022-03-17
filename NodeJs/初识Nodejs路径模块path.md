## 引入模块
NodeJs中的path模块是用于处理文件/目录路径的一个内置模块

`const path = require('oath')`

## __dirname和__filename
- `__dirname`可以看作nodejs中的全局变量，他始终表示当前执行文件所在目录的完整路径（绝对路径）
- `__filename`可以看作是nodejs中的全局变量，始终表示当前执行文件的完整文件名（绝对路径，包括文件名）

```javascript
// 当前执行文件的完整路径为\Stone\node\node\path_module\test.js
const path = require("path");

console.log(__dirname); 
// \Stone\node\node\path_module
console.log(__filename); 
// \Stone\node\node\path_module\test.js
```
然后我们在 `\Stone\node\node`目录下运行`node path_module\test.js`，会发现输出结果同上， 所以这就是说明 __dirname 和 __filename 始终跟当前执行文件有关，跟启动脚本所在目录无关。

## 拼接路径 join
`path.join([...paths])`
- paths:路径片段
- 返回值：将所有路径片段连接在一起后生成的路径

注意：
- 如果paths不是字符串片段，则抛出TypeError
- 零长度的path片段会被忽略
- 如果链接后的路径字符长度为0，则返回`'.'`，表示当前目录
- `'./'和'../'`在路径中会计算生效

```javascript
const path = require("path");

path.join('') // '.'
path.join('./') // '.\'
path.join('../') // '..\'
path.join('/foo/','bar','baz','../','index.js') // '\foo\bar\index.js'
path.join('./bar','baz' ,'/','../','',index.js') // 'bar\index.js'
path.join('foo', {}, 'bar'); // 'TypeError: Path must be a string. Received {}'
```


## 获取路径基础名 basename
`path.basename(path[,ext])`
- path:文件/目录路径
- ext：可选，文件扩展名（.js .css等）
- 返回值：path路径的最后一部分

注意
- 如果path不是字符串或给定的ext参数不是字符串，则抛出TypeError
- 如果有ext参数，当ext后缀名与文件名匹配上时，返回的文件名会省略文件后缀
- 如果path尾部有目录分隔符则会被忽略

```javascript
const path = require("path");

path.basename('./ext/test.js') //test.js
path.basename('./ext/test.js','.js') //test (当后缀名与文件名匹配上时返回的文件名会省略文件后缀)
path.basename('./ext/test.js','.html') //test.js (没有匹配上时返回文件全名)
path.basename('./ext/foo/') // foo (尾部目录分隔符被忽略)

```

## 获取路径扩展名 extname
既然有获取路径的基础名，与之对应就同样存在获取路径的扩展名

`path.extname(path)`
- path：文件/目录路径
- 返回值：path路径的扩展名，从最后一次出现`'.'`字符到path最后一部分的字符串结束，若无扩展名则返回空

```javascript
const path = require("path");

path.extname('foo/bar/baz/test.js'); // .js
path.extname('foo/bar/baz');// '' (无扩展名返回 '')
path.extname('foo/bar/baz/.'); // ''
path.extname('foo/bar/baz/test.'); // '.'
path.extname('foo/bar/baz/.test'); // ''
path.extname('foo/bar/baz/.test.js'); // '.js'
```

## 获取路径目录名 dirname
`path.dirname(path)`
- path:文件/目录路径
- 返回值：path路径的目录名

注意
- 如果path不是字符串，则抛出TypeError
- 如果path尾部有目录分隔符'`/'`则会被忽略

```javascript
const path = require("path");

path.dirname('./foo/bar/baz'); //./foo/bar (相对路径/绝对路径均可)
path.dirname('/foo/bar/baz/'); // /foo/bar (尾部目录分隔符被忽略)
path.dirname('/foo/bar/baz/test.js'); // /foo/bar/baz
```

## 解析路径 parse
`path.parse(path)`
- path：文件/目录路径
- 返回值：带有属性（dir,root,base,name,ext）的对象
- root：根目录
- dir：文件所在的文件夹
- base：完整文件
- name：文件名
- ext：文件后缀名

注意
- 如果path不是字符串，则抛出TypeError
- 如果尾部有目录分隔符则会被忽略

```
┌──────────────────┬────────────┐
│          dir     │    base    │
├──────┬           ├──────┬─────┤
│ root │           │ name │ ext │
"  /    foo/bar/baz/ index  .js "
```

```javascript
const path = require("path");

path.parse('/foo/bar/baz/index.js')
// {
//     root: '/',
//     dir: '/foo/bar/baz',
//     base: 'index.js',
//     ext: '.js',
//     name: 'index'
//   }

path.parse('/foo/bar/baz') //尾部目录分隔符省略
// {
//     root: '/',
//     dir: '/foo/bar',
//     base: 'baz',
//     ext: '',
//     name: 'baz'
//   }

path.parse('./foo/bar/baz/index.js') //当路径为相对路径 ./ 或../时 解析结果中root(代表根目录,绝对路径才有值)为 ''
// {
//     root: '',
//     dir: './foo/bar/baz',
//     base: 'index.js',
//     ext: '.js',
//     name: 'index'
//   }

```

## 参考文档
[nodejs — path 模块](https://juejin.cn/post/6844903972256350222#heading-14)