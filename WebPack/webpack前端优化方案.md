## 1.JS代码压缩
`terser`是一个JavaScript的编译、压缩的工具集，可以帮助我们压缩、优化我们的代码，让包变得更小。

在production模式下，webpack默认就是使用`TerserPlugin`来处理的，如果想要自定义配置，方法如下：

```javascript
const TerserPlugin = require('terser-webpack-plugin')
module.exports = {
    ...
    optimization: {
        minimize: true,
        minimizer: [
            new TerserPlugin({
                parallel: true // 电脑cpu核数-1
            })
        ]
    }
}
```

**属性**

- **extractComments**：默认值为true，表示会将注释抽取到一个单独的文件中，开发阶段，我们可设置为 false ，不保留注释
- **parallel**：使用多进程并发运行提高构建的速度，默认值是true，并发运行的默认数量： os.cpus().length - 1
- **terserOptions**：设置我们的terser相关的配置：
- **compress**：设置压缩相关的选项，mangle：设置丑化相关的选项，可以直接设置为true
- **mangle**：设置丑化相关的选项，可以直接设置为true
- **toplevel**：底层变量是否进行转换
- **keep_classnames**：保留类的名称
- **keep_fnames**：保留函数的名称


## 2.CSS代码压缩
CSS压缩通常是去除无用的空格等，很难去修改选择器、属性的名称、值等。

CSS的压缩我们可以使用另外一个插件`css-minimizer-webpack-plugin`

```javascript
npm install css-minimizer-webpack-plugin -D
```

**配置方法**

```javascript
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')
module.exports = {
    // ...
    optimization: {
        minimize: true,
        minimizer: [
            new CssMinimizerPlugin({
                parallel: true
            })
        ]
    }
}
```

## 3.HTML代码压缩
使用`HtmlWebpackPlugin`插件来生成HTML模板的时候，通过配置`minify`进行html优化

```javascript
module.exports = {
    ...
    plugin:[
        new HtmlwebpackPlugin({
            ...
            minify:{
                minifyCSS:false, // 是否压缩css
                collapseWhitespace:false, // 是否折叠空格
                removeComments:true // 是否移除注释
            }
        })
    ]
}
```

设置了`minify`，实际会使用另一个插件`html-minifier-terser`

## 4.文件大小压缩
对文件的大小进行压缩，减少http传输过程中带宽的损耗

```javascript
npm install compression-webpack-plugin -D
```

```javascript
new ComepressionPlugin({
    test:/\.(css|js)$/,  // 哪些文件需要压缩
    threshold:500, // 设置文件多大开始压缩
    minRatio:0.7, // 至少压缩的比例
    algorithm:"gzip", // 采用的压缩算法
})
```

## 5.图片压缩
一般来说在打包之后，一些图片文件的大小是远远要比 js 或者 css 文件要来的大，所以图片压缩较为重要

**配置方法**

```javascript
module: {
  rules: [
    {
      test: /\.(png|jpg|gif)$/,
      use: [
        {
          loader: 'file-loader',
          options: {
            name: '[name]_[hash].[ext]',
            outputPath: 'images/',
          }
        },
        {
          loader: 'image-webpack-loader',
          options: {
            // 压缩 jpeg 的配置
            mozjpeg: {
              progressive: true,
              quality: 65
            },
            // 使用 imagemin**-optipng 压缩 png，enable: false 为关闭
            optipng: {
              enabled: false,
            },
            // 使用 imagemin-pngquant 压缩 png
            pngquant: {
              quality: '65-90',
              speed: 4
            },
            // 压缩 gif 的配置
            gifsicle: {
              interlaced: false,
            },
            // 开启 webp，会把 jpg 和 png 图片压缩为 webp 格式
            webp: {
              quality: 75
            }
          }
        }
      ]
    },
  ]
} 

```


## 6.Tree Shaking
`Tree Shaking`是一个术语，在计算机中表示消除死代码，依赖于`ES module`的静态语法分析

在webpack实现`Tree Shaking`有两种不同的方案
- **usedExports**：通过标记某些函数是否被使用，之后通过`Terser`来进行优化
- **sideEffects**：跳过整个模块/文件，直接查看该文件是否有副作用

两种方案有不同的效果

### 6.1.usedExoprts
配置方法很简单，只需要将`usedExports`设为`true`

```javascript
module.exports = {
    ...
    optimization:{
        usedExports
    }
}
```

使用之后，没被用上的代码在webpack打包中会加入`unused harmony export mul`注释，用来告知`Terser`在优化时，可以删除掉这段代码

### 6.2.sideEffects
`sideEffects`用于告知webpack compiler哪些模块时有副作用，配置方法是在package.json中设置`sideEffects`属性

如果`sideEffects`设置为`false`，就是告知webpack可以安全的删除未用到的`exports`

如果有些文件需要保留，可以设置为数组的形式

```javascript
"sideEffecis":[
    "./src/util/format.js",
    "*.css" // 所有的css文件
]
```

### 6.3.CSS Tree Shaking
上面`6.1`和`6.2`都是关于JS的`Tree Shaking`，CSS同样也能够实现

CSS进行`Tree Shaking`优化可以安装**PurgeCss**插件

```javascript
npm install purgecss-plugin-webpack -D
```

```javascript
const PurgeCssPlugin = require('purgecss-webpack-plugin')
module.exports = {
    ...
    plugins:[
        new PurgeCssPlugin({
            path:glob.sync(`${path.resolve('./src')}/**/*`), {nodir:true}// src里面的所有文件
            satelist:function(){
                return {
                    standard:["html"]
                }
            }
        })
    ]
}
```
- paths：表示要检测哪些目录下的内容需要被分析，配合使用glob
- 默认情况下，Purgecss会将我们的html标签的样式移除掉，如果我们希望保留，可以添加一个safelist的属性

## 7.代码分离
将代码分离到不同的bundle中，之后我们可以按需加载，或者并行加载这些文件

默认情况下，所有的JavaScript代码（业务代码、第三方依赖、暂时没有用到的模块）在首页全部都加载，就会影响首页的加载速度

代码分离可以分出出更小的bundle，以及控制资源加载优先级，提供代码的加载性能

这里通过`splitChunksPlugin`来实现，该插件webpack已经默认安装和集成，只需要配置即可

默认配置中，chunks仅仅针对于异步（async）请求，我们可以设置为initial或者all

```javascript
module.exports = {
    ...
    optimization:{
        splitChunks:{
            chunks:"all"
        }
    }
}

```
`splitChunks`主要属性有如下：

- **Chunks**：对同步代码还是异步代码进行处理
- **minSize**： 拆分包的大小, 至少为minSize，如何包的大小不超过minSize，这个包不会拆分
- **maxSize**： 将大于maxSize的包，拆分为不小于minSize的包
- **minChunks**：被引入的次数，默认是1

## 8.内联chunk
可以通过`InlineChunkHtmlPlugin`插件将一些chunk的模块内联到html，如runtime的代码（对模块进行解析、加载、模块信息相关的代码），代码量并不大，但是必须加载的

```javascript
const InlineChunkHtmlPlugin = require('react-dev-utils/InlineChunkHtmlPlugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
module.exports = {
    ...
    plugin:[
        new InlineChunkHtmlPlugin(HtmlWebpackPlugin,[/runtime.+\.js/]
}

```

## 参考文档
[webpack如何优化前端性能](https://blog.csdn.net/thewar196/article/details/122801669)