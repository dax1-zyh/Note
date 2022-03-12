## watch
watch()接收三个参数，**监听对象，响应事件，配置**


```javascript
<template>
  <h2>当前求和为{{ sum }}</h2>
  <button @click="sum++">点我+1</button>
  <hr>
  <h2>当前信息为:{{ msg }}</h2>
  <button @click="msg+='!'">修改信息</button>
  <hr>
  <h2>姓名：{{ person.name }}</h2>
  <h2>年龄：{{ person.age }}</h2>
  <h2>薪资：{{ person.job.j1.salary }}k</h2>
  <button @click="person.name+='~'">修改姓名</button>
  <button @click="person.age++">增长年龄</button>
  <button @click="person.job.j1.salary++">涨薪</button>
</template>

<script>
import {reactive, ref, watch} from 'vue'

export default {
  setup() {
    const sum = ref(0)
    const msg = ref('你好啊')
    const person = reactive({
      name: 'frank',
      age: 18,
      job: {
        j1: {
          salary: 20
        }
      }
    })

    // 情况一：监视ref所定义的一个响应式数据
    watch(sum, (newValue, oldValue) => {
      console.log('sum变了', newValue, oldValue)
    }, {immediate: true})

    // 情况二：监视ref所定义的多个响应式数据
    watch([sum, msg], (newValue, oldValue) => {
      console.log('sum或msg变了', newValue, oldValue)
    }, {immediate: true})

    // 情况三：监视reactive所定义的一个响应式数据的全部属性
    // 1.注意：此处无法正确的获得oldValue
    // 2.注意：强制开启了深度监视(deep配置无效)
    watch(person, (newValue, oldValue) => {
      console.log('person变化了', newValue, oldValue)
    })

    // 情况四：监视reactive所定义的一个响应式数据中的某个数据
    // 此时要监听的对象需要以函数的返回值形式提供
    watch(() => person.name, (newValue, oldValue) => {
      console.log('person的name变化了', newValue, oldValue)
    })

    // 情况五：监视reactive所定义的一个响应式数据中的某些数据
    watch([() => person.name, () => person.age], (newValue, oldValue) => {
      console.log('person的name或age变化了', newValue, oldValue)
    })

    // 特殊情况，由于监视的是reactive所定义的对象中的某个属性，因此deep配置有效
    watch(() => person.job, (newValue, oldValue) => {
      console.log('person的name或age变化了', newValue, oldValue)
    })

    return {
      sum,
      msg,
      person
    }
  }
}
</script>

```

### 注意

 - 监听reactive直接定义的响应式数据时，`oldValue`无法正确获取，强制开启了深度监视（deep配置无效）
 - 监听reactive定义的响应式数据中的某个属性时，监听对象要以函数返回值的形式提供，deep配置有效


## watchEffect
watchEffect()只接收一个回调函数作为参数。立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。

即watchEffect所指定的回调中用到的数据只要发生变化，则重新执行回调
```javascript
watchEffect(() => {
	const x1 = sum.value
	const x2 = person.age
	console.log('watchEffect配置的回调执行了')
})
```

## watch vs watchEffect

 - watch：既要指明监听的属性，也要指明监听的回调
 - watchEffect：不用指明监听哪个属性，监听的回调中用到哪个属性，就监听哪个属性
 - watchEffect与computed有相似的地方，都随所依赖的数据变化而变化，但computed更注重计算出来的结果（必须有返回值），watchEffect更注重过程，不用写返回值