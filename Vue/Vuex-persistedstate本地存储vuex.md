## 使用场景

最近在做Vue项目中的登录模块，登陆成功后获取到token，将token存储在vuex中，然而我发现切换路由后vuex中的数据都恢复默认了，原来页面刷新之后vuex的数据都会恢复默认。而后面进行鉴权处理需要token，于是我们要将vuex中的数据进行本地存储。

这里就用到了vuex持久化插件`vuex-persistedstate`



## Vuex-persistedstate

这个插件的原理结合了存储方式，只是统一配置后就不需要手动写存储方法了



使用方法：

- 安装

  ```
  npm install vuex-persistedstate --save
  ```

- 在store下的index.js中引入配置

  ```js
  import { createStore } from 'vuex'
  import createPersistedState from 'vuex-persistedstate'
  
  export default createStore({  
      state: {  },  
      mutations: {  },  
      actions: {  },  
      modules: {  },  
      plugins: [    
          createPersistedState(),  
      ],
  })
  
  ```

  这样是默认存储到localStorage，如果想要存储到sessionStorage，配置如下

  ```js
  import { createStore } from 'vuex'
  import createPersistedState from 'vuex-persistedstate'
  export default createStore({  
      state: {  },  
      mutations: {  },  
      actions: {  },  
      modules: {  },  
      plugins: [    
          // 把vuex的数据存储到sessionStorage    
          createPersistedState({      
              storage: window.sessionStorage,    
          }),  
      ],
  })
  ```

  ​	默认持久化所有的state，如果想要存储指定的state，配置如下

  ```js
  import { createStore } from 'vuex'
  import createPersistedState from 'vuex-persistedstate'
  export default createStore({  
      state: {  },  
      mutations: {  },  
      actions: {  },  
      modules: {  },  
      plugins: [    
          // 把vuex的数据存储到sessionStorage    
          createPersistedState({      
              storage: window.sessionStorage,      
              reducer(val) {        
                  return {          
                      // 只存储state中的userData          
                      userData: val.userData        
                  }      
              }    
          }),  
      ],
  })
  ```



**API**

key: 存储持久状态的key（默认vuex）

paths ：部分持续状态的任何路径的数组。如果没有路径给出，完整的状态是持久的。（默认：[]）

reducer ：一个函数，将被调用来基于给定的路径持久化的状态。默认包含这些值。

subscriber ：一个被调用来设置突变订阅的函数。默认为store => handler => store.subscribe(handler)

storage ：而不是（或与）getState和setState。默认为localStorage。

getState ：将被调用以重新水化先前持久状态的函数。默认使用storage。

setState ：将被调用来保持给定状态的函数。默认使用storage。

filter ：将被调用来过滤将setState最终触发存储的任何突变的函数。默认为() => true



参考链接

 [Vuex持久化插件（vuex-persistedstate）解决页面刷新vuex数据丢失的问题](https://juejin.cn/post/6934947947746426910)

