- ## params参数
  - 路由链接（携带参数）：`<Link to = {'/demo/test/tom/18'}> </Link >`
  - 注册路由（声明接收）：`<Route path= '/demo/test/:name/:age' component={Test} />`
  - 接收参数：`this.props.match.params`
  - 缺点：只能传字符串，不能传递对象，参数多导致url过长

  ## search参数
  - 路由链接（携带参数）：`<Link to = {'/demo/test?name='tom'&age=18'}></Link>`
  - 注册路由（无需声明接收）：`<Route path='/demo/test' component={Test} />`
  - 接收参数：`this.props.location.search`
  - 备注：接收到的search是`urlencoded`编码字符串，需要qs解析
  - 缺点：只能传字符串，不能传递对象，参数多导致url过长

  ## query参数
  - 路由链接（携带参数）：`<Link to = {{ pathname: '/demo/test' , query: {name: 'tom',age:18} }}></Link>`
  - 注册路由（无需声明接收）：`<Route path='/demo/test' component={Test} />`
  - 接收参数：`this.props.location.query`
  - 优点：可以传递对象
  - 缺点：`HashRouter`刷新地址栏，参数丢失，`BrowserRouter`不会丢失参数


  ## state参数
  - 路由链接（携带参数）：`<Link to = {{ pathname: '/demo/test' , state: {name:'tom',age:18} }}></Link>`
  - 注册路由（无需声明接收）：`<Route path='/demo/test' component={Test} />`
  - 接收参数：`this.props.location.state`
  - 优点：可以传递对象
  - 缺点：`HashRouter`刷新地址栏，参数丢失，`BrowserRouter`不会丢失参数