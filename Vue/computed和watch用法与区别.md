## computed

简单用法，只有getter，只需读取，不需要修改。

```javascript
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
```

完整写法，有getter和setter，需要对数据进行修改

```javascript
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```

**注意：computed必须有return，同时因为return是同步执行结果，因此computed中不能执行异步任务**

------
## watch
简单用法，函数写法，只有handler

```javascript
  watch: {
    firstName(newName, oldName) {
      this.fullName = newName + ' ' + this.lastName;
    }
  } 
```

监听对象接收两个参数，新值和旧值。
不需要return，可以执行异步操作。
当不需要使用其他功能，例如下面的immediate和deep属性时，可以用上方的函数写法，否则要用完整写法，即对象写法。

### immediate属性
上面的例子是值变化时候，watch才执行，我们想让值最初时候watch就执行就用到了handler和immediate属性

```javascript
watch: {
  firstName: {
    handler(newName, oldName) {
      this.fullName = newName + ' ' + this.lastName;
    },
    // 代表在wacth里声明了firstName这个方法之后立即先去执行handler方法，如果设置了false，那么效果和上边例子一样
    immediate: true
  }
}
```

### deep属性
深度监听，常用于对象下面的属性的改变

```javascript
export default {
  data: () => ({
    obj: {
      hello: 'james'
    }
  }),
  watch: {
    obj: {
      handler(newVal, oldVal) {
        console.log(`obj changed: ${newVal}`);
      },
      immediate: true,
      deep: true
    }
  }
};
```
deep即深入观察, 监听器会层层遍历, 给对象的所有属性(及子属性)添加监听器. 这样做无疑会有很大的性能开销, 修改obj中任何一个属性都会触发监听器中的处理函数.

若只想监听obj中的某个属性, 可以使用字符串监听形式.

```javascript
export default {
  data: () => ({
    obj: {
      hello: 'james'
    }
  }),
  watch: {
    'obj.hello': {
      handler(newVal, oldVal) {
        console.log(`obj changed: ${newVal}`);
      },
      immediate: true,
      deep: false
    }
  }
};
```

-------

## computed和watch区别

 - **功能上**：computed是**计算属性**，也就是依赖其它的属性计算所得出最后的值。watch是去**监听**一个值的变化，然后执行相对应的函数
 - **使用上**：computed中的函数必须要用return返回（默认走get函数）；watch的回调里面会传入监听属性的**新旧值**，通过这两个值可以做一些特定的操作，比如**异步任务**，不是必须要用return
 - **性能上**：computed中的函数所**依赖**的属性没有发生变化，那么调用当前的函数的时候会从**缓存**中读取，而watch在每次监听的值发生变化的时候都会执行回调
 - **场景上**：computed：当**一个数据受多个数据影响**的时候，例子：购物车金额结算；watch：当**一条数据影响多条数据**，例子：搜索框。或者**需要执行异步任务**的时候