## 什么是并发控制
假设有6个待办任务要执行，而我们希望限制同时执行的任务个数，即最多只有2个任务能同时执行。当正在执行任务列表中的任何1个任务完成后，程序会自动从待办任务列表中获取新的待办任务并把该任务添加到正在执行任务列表中。

### 图解
![在这里插入图片描述](https://img-blog.csdnimg.cn/920c2de4e95e432fad15c1e0dc2ba483.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/e9a6f24f21c54df7bfc16c73ab25d584.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/59c52d491da940d9bc881db5c01f2a16.png)


## async-pool
本文使用了[async-pool](https://github.com/rxaviers/async-pool)库来实现并发控制

## async-pool的使用

```javascript
 const timeout = i => new Promise(resolve => setTimeout(() => resolve(i), i))
 await asyncPool (5,[1000,2000,3000,4000],timeout)
```

其中，`asyncPool`是实现并发控制的主体函数，形式如下
`function asyncPool(poolLimit, array, iteratorFn){ ... }`

### 参数介绍

- `poolLimit`（Number）：表示限制的并发数；
- `array`（Array）：表示任务数组；
- `iteratorFn`（FunctionaAz）：表示迭代函数，用于实现对每个任务项进行处理，该函数会返回一个 Promise 对象或异步函数。

## async-pool的实现
实现的核心是通过`Promise.all()`和`Promise.race()`配合

```javascript
        async function asyncPool (poolLimit,array,iteratorFn) {
            const ret = [] // 存储所有的异步任务
            const executing = [] // 存储所有正在执行的任务
            for (const item of array) {
                const p = Promise.resolve().then(() => iteratorFn(item,array)) 
                // 调用iteratorFn函数创建异步任务
                ret.push(p)
                // 保存新的异步任务

                if (poolLimit <= array.length) {
                    // 当poolLimit小于等于总任务数量时，进行并发控制
                    const e = p.then(() => executing.splice(executing.indexOf(e),1))
                    // 当任务完成后，从正在执行的任务队列中移除任务，腾出一个空位
                    executing.push(e)
                    // 加入正在执行的异步任务

                    if (executing.length >= poolLimit) {
                        await Promise.race(executing)   
                        // 有任务执行完成之后，进入下一次循环
                    }
                }
            }
            return Promise.all(ret) // 所有任务完成之后返回
        }
```


## 参考文档
[JavaScript 中如何实现并发控制？](https://juejin.cn/post/6976028030770610213)