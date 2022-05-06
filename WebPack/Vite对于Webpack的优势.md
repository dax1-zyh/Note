## 什么是Vite
Vite是一种新型前端构建工具
Vite在大型项目开发模式下，打包速度远高于webpack

## 为什么Vite可以这么快
Vite性能优势主要来自对打包流程进行了优化

优化有如下三点

### 1.快速冷启动

Vite只启动一台静态页面的服务器，不会打包全部项目文件代码，服务器根据客户端的请求加载不同的模块处理，实现按需加载。

![在这里插入图片描述](https://img-blog.csdnimg.cn/3c4093ceabc9426bbe9f6ddb2bd3bdaa.png)
而我们所熟知的webpack则是，一开始就将整个项目都打包一遍，再开启`dev-server`，如果项目规模庞大，打包时间必然很长。

![在这里插入图片描述](https://img-blog.csdnimg.cn/57cffe1e825c4fe786efbc10d3f57dac.png)


## 2.打包编译速度
Vite使用esbuild预构建依赖，而esbuild是用`Go`编写的，比`JS`编写的打包器快很多。

## 3.热模块更新
对于热更新问题，Vite采用`立即编译当前修改文件`的办法，同时结合vite的缓存机制（http缓存 ==》 Vite内置缓存），加载更新后的文件内容