昨天已经成功地获取到登录的token，并将token存储在session storage中，今天完成了获取到token之前与之后内容页与登录页之间的路由跳转，主要用路由守卫来实现



- 在 `router/index.js`中，将要进行拦截的页面meta中加入 `loginRequest: 'true'`，冒号前面的变量可以自己定义，这步是为后面判断是否拦截提供依据

  ```js
    {
      path: '/',
      redirect: '/index',
      component: Layout,
      meta: { title: '主页' },
      children: [{
        path: 'index',
        name: 'Index',
        meta: { title: '主页', loginRequest: 'true', hideSidebar: true, breadcrumb: false, icon: 'el-icon-menu' },
        component: () => import('@/views/home/index')
      }]
    },
  ```

- 在`main.js`文件里面添加全局路由守卫（记得放在vue初始化之前，否则首次进入网页不会拦截）

  ```js
  function getToken() {	// 返回token
    const ssrObj = JSON.parse(sessionStorage.getItem('vuex'))
    if (ssrObj) {
      return ssrObj.access_token
    } else {
      return
    }
  }
  
  
  router.beforeEach((to, from, next) => {
    if (to.meta.loginRequest) { // 如果路由meta中loginRequest为true，进行拦截
      if (getToken()) {
        next() // 如果已经登陆可直接进入页面
      } else { // 否则跳入登录页，并且记住要跳入的页面，以方便登陆完成后直接进入该页面
        next({
          path: '/login',
          query: {
            redirect: to.fullPath // 把要跳转的页面路径作为参数传到登录页面
          }
        })
      }
    } else {
      next() // 直接进入页面
    }
  })
  ```

- 在登录页.vue ,进行路由的跳转，我直接添加在了登录请求的成功promise上

  ```js
  .then(res => {
          this.$store.commit('getToken', res.data.access_token)
          if (res.data.access_token) {
            this.$message({
              showClose: true,
              message: '登陆成功',
              type: 'success'
            })
          }
          const redirect = decodeURIComponent(this.$route.query.redirect || '/') // 获得路由携带的参数
          this.$router.push({ path: redirect }) // 路由跳转
        })
  ```

  