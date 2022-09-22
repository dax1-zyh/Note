## $router 和 $route
- `$router`是Vue-router的路由实例，想要导航到不同的url，需要使用`$router.push`方法
- `$route`是当前router的跳转对象，可以获取到`name、path、query、params`等信息

## query

`query`传参，可以通过`name`和`path`两种形式传递，

```javascript

//query传参，使用name跳转
this.$router.push({
    name:'second',
    query: {
        queryId:'20180822',
        queryName: 'query'
    }
})
 
//query传参，使用path跳转
this.$router.push({
    path:'second',
    query: {
        queryId:'20180822',
        queryName: 'query'
    }
})
 
//query传参接收
this.queryName = this.$route.query.queryName;
this.queryId = this.$route.query.queryId;
```

生成的`url`如图所示：携带`？`的参数形式，并且包含参数的`key`和`value`
![在这里插入图片描述](https://img-blog.csdnimg.cn/27beca1db6d74da688332bd68b057d8b.png)

## params
`params`传参，只支持`name`的形式

```csharp

//params传参 使用name
this.$router.push({
  name:'second',
  params: {
    id:'20180822',
     name: 'query'
  }
})
 
//params接收参数
this.id = this.$route.params.id ;
this.name = this.$route.params.name ;
 
//路由
{
path: '/second/:id/:name',
name: 'second',
component: () => import('@/view/second')
}
```

`params`方式需要注意的是需要定义路由信息如：`path: '/xx/:id/:name'`,这样才能进行携带参数跳转，否则`url`不会进行变化，并且再次刷新页面后参数会读取不到

生成的url如图所示：只包含参数的`value`，安全性相对较高
![在这里插入图片描述](https://img-blog.csdnimg.cn/1017e8568c0449dc89af327ad327a470.png)


## 总结
- 接收参数的形式不一样，分别为`this.$route.query.id`和`this.$route.params.id`
- `query`支持`path和name`两种形式，`params`仅支持`name`形式
- 生成`url`的格式不同，`query`产生的url带问号且包含参数的`key和value`，`params`仅携带参数的`value`
- 使用`params`需要将路由设置为`path:/xx/:id/:name`的形式