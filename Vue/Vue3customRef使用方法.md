## 使用场景
`ref`函数可以创建一个响应式数据，有时我们希望可以自己控制数据的触发响
应，就可以使用`customRef`函数自定义一个`ref`

## 写法
自定义`ref`的本质就是返回一个`customRef`函数，即`rerturn customRef()`

`customRef`接收两个参数

 - 用于追踪的`track`，通知Vue去追踪value的变化
 - 用于触发响应的`trigger`，通知Vue去重新解析模板

`customRef`函数需要返回带有`get和set`和对象


## 举个栗子
官方例子，带防抖功能的自定义ref

```javascript
import {customRef} from "vue";

export default {
  setup() {
    // 自定义一个ref，名为myRef
    function myRef(value, delay) {
      let timeout
      return customRef((track, trigger) => {
        return {
          get() {
            console.log(`有人从myRef这个容器中读取数据了，把${value}给他`)
            track() // 通知Vue追踪value的变化
            return value
          },
          set(newValue) {
            console.log(`有人把myRef这个容器中数据改为了${newValue}`)
            timeout = setTimeout(() => {
              value = newValue
              trigger() // 通知Vue去重新解析模板
            }, delay)
          }
        }
      })
    }

    const word = myRef('hello', 500)

    return {
      word,
    }
  }
}
```