## 导入模块
`const http = require('http')` 

## http服务器
### 创建一个简单的http服务器
```javascript
// index.js
const http = require('http')

// 创建http服务器实例
const server = http.createServer()

// 当有人请求目标地址时触发request，同时调用回调函数
server.on('request',(request,response) => {
    console.log('有人请求了服务器')
})

// 开启服务器，监听80端口
server.listen(80,() => {
    console.log('成功启动服务器 http://localhost:80')
})
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/581ae2f3525f41dc8b51b6c0ec82e26a.png)
### request对象和response对象
- request对象封装了http请求，调用request的属性和方法可以拿到所有http请求的信息，例如url，method
- response对象封装了http响应，操作response的方法，可以把http相应返回给浏览器，例如`res.end()`向服务器发送指定内容并结束这次请求，`res.setHeader`设置响应头信息

`res.setHeader('Content-Type','text/html;charset=utf-8')`可以防止中文乱码

### 关闭服务器
`server.close(()=>{ })`

## http客户端
http模块提供了两个函数`http.request()`和`http.get()`，功能是作为客户端向httpo服务器发起请求

### http.request()
`http.request(option,callback)`接收两个参数
- option可以是一个对象，包含请求的参数；也可以是字符串，就表示这是一个URL，node会自动调用url.parse来解析这个参数
- callback为请求的回调函数，需要接收一个参数

http.request()返回一个http.ClientRequest类的实例。它是一个可写数据流，如果你想通过POST方法发送一个文件，可以将文件写入这个ClientRequest对象

需要在请求之后调用`req.end()`来结束请求。

```javascript
const http = require('http')
let options = {
    hostname: 'www.example.com',
    port: 80,
    path: '/',
    method: 'GET'
}

const req = http.request(options, (res) => {
    console.log(`STATUS: ${res.statusCode}`) //返回状态码
    console.log(`HEADERS: ${JSON.stringify(res.headers, null, 4)}`) // 返回头部
    res.setEncoding('utf8') // 设置编码
    res.on('data', (chunk) => { //监听 'data' 事件
        console.log(`主体: ${chunk}`)
    })

})
req.end() // end方法结束请求
```
### http.get()
类似于 http.request()，但会自动地设置 HTTP 方法为 GET，并自动地调用 `req.end()`。