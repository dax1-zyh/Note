## keep-alive 用法

`keep-alive`是Vue内置的一个组件，可以使被包含的组件保留状态，避免**重新渲染**



用法

```js
<keep-alive>
  <component>
    <!-- 该组件将被缓存！ -->
  </component>
</keep-alive>
```



## 使用场景

例如在A组件内多个输入框中输入信息，此时需要跳转进入B组件进行操作，再返回A组件。若没有使用 `keep-alive`，则A组件内输入框中的信息都会被清空，因为A组件被重新渲染。使用 `keep-alive`，则会保留之前输入框中的信息



## props

- `include` ，只有匹配的组件才会被缓存
- `exclude`，任何匹配的组件都不会被缓存



```js
// 组件 a
export default {
  name: 'a',
  data () {
    return {}
  }
}
```

```js
// include
<keep-alive include="a">
  <component>
    <!-- name 为 a 的组件将被缓存！ -->
  </component>
</keep-alive>可以保留它的状态或避免重新渲染

// exclude
<keep-alive exclude="a">
  <component>
    <!-- 除了 name 为 a 的组件都将被缓存！ -->
  </component>
</keep-alive>可以保留它的状态或避免重新渲染
```



## 只缓存 `router-view`中的某个组件 ？

使用`include`配合`router.meta`

```js
// routes 配置
export default [
  {
    path: '/',
    name: 'home',
    component: Home,
    meta: {
      keepAlive: true // 需要被缓存
    }
  }, {
    path: '/:id',
    name: 'edit',
    component: Edit,
    meta: {
      keepAlive: false // 不需要被缓存
    }
  }
]
```

```js
<keep-alive>
    <router-view v-if="$route.meta.keepAlive">
        <!-- 这里是会被缓存的视图组件，比如 Home！ -->
    </router-view>
</keep-alive>

<router-view v-if="!$route.meta.keepAlive">
    <!-- 这里是不被缓存的视图组件，比如 Edit！ -->
</router-view>
```



## 拓展使用场景

假设这里有 3 个路由： A、B、C。

需求：

- 默认显示 A
- B 跳到 A，A 不刷新
- C 跳到 A，A 刷新



实现方式：

- 在 A 路由里面设置 meta 属性：

  ```js
  {
          path: '/',
          name: 'A',
          component: A,
          meta: {
              keepAlive: true // 需要被缓存
          }
  }
  ```

- 在 B 组件里面设置 beforeRouteLeave：

  ```js
  export default {
          data() {
              return {};
          },
          methods: {},
          beforeRouteLeave(to, from, next) {
               // 设置下一个路由的 meta
              to.meta.keepAlive = true;  // 让 A 缓存，即不刷新
              next();
          }
  };
  ```

- 在 C 组件里面设置 beforeRouteLeave：

  ```js
  export default {
          data() {
              return {};
          },
          methods: {},
          beforeRouteLeave(to, from, next) {
              // 设置下一个路由的 meta
              to.meta.keepAlive = false; // 让 A 不缓存，即刷新
              next();
          }
  };
  
  ```

  

这样便能实现 B 回到 A，A 不刷新；而 C 回到 A 则刷新。





部分搬运 [vue-router 之 keep-alive](https://www.jianshu.com/p/0b0222954483)