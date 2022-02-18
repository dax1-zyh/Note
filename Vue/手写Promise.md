## 手写Promise

**Promise.js**

```js
// 声明构造函数
function Promise(executor) {
    // 添加属性
    this.PromiseState = 'pending';
    this.PromiseResult = null;
    const self = this
    // 声明属性
    this.callbacks = [];

    // resolve函数
    function resolve(data) {
        // 判断状态(若状态不为pending，直接返回)
        if (self.PromiseState !== 'pending') return
        // 修改状态（PromiseState)
        self.PromiseState = 'fulfilled';
        // 设置对象结果值(PromiseResult)
        self.PromiseResult = data;
        // 调用成功的回调函数
        setTimeout(() => {
            self.callbacks.forEach(item => {
                item.onResolved(data)
            })
        })
    }

    // reject函数
    function reject(data) {
        // 判断状态(若状态不为pending，直接返回
        if (self.PromiseState !== 'pending') return
        // 修改状态（PromiseState)
        self.PromiseState = 'rejected';
        // 设置对象结果值(PromiseResult)
        self.PromiseResult = data;
        // 调用失败的回调函数
        setTimeout(() => {
            self.callbacks.forEach(item => {
                item.onRejected(data)
            })
        })
    }

    try {
        // 同步调用 执行器函数
        executor(resolve, reject)
    } catch (e) {
        reject(e)
    }
}

// 添加then方法
Promise.prototype.then = function (onResolved, onRejected) {
    const self = this

    // 判断回调函数参数
    // 异常穿透
    if (typeof onRejected !== 'function') {
        onRejectd = reason => {
            throw reason
        }
    }
    // 传值
    if (typeof onResolved !== 'function') {
        onResolved = value => value
        // value => {return value}
    }

    return new Promise((resolve, reject) => {
        // 封装函数
        function callback(type) {
            try {
                // 执行成功的回调函数
                let result = type(self.PromiseResult)
                // 判断
                if (result instanceof Promise) {
                    result.then(v => {
                        resolve(v)
                    }, r => {
                        reject(r)
                    })
                } else {
                    resolve(result)
                }
            } catch (e) {
                reject(e)
            }
        }

        // 调用回调函数
        if (this.PromiseState === 'fulfilled') {
            setTimeout(() => {
                callback(onResolved);
            })
        }
        if (this.PromiseState === 'rejected') {
            setTimeout(() => {
                callback(onRejected);
            })
        }
        if (this.PromiseState === 'pending') {
            // 保存回调函数
            this.callbacks.push({
                onResolved: function () {
                    callback(onResolved)
                },
                onRejected: function () {
                    callback(onRejected)
                }
            })
        }
    })
}

// 添加catch方法
Promise.prototype.catch = function (onRejected) {
    return this.then(undefined, onRejected)
}

// 添加resolve方法
Promise.resolve = function (value) {
    return new Promise((resolve, reject) => {
        if (value instanceof Promise) {
            value.then(v => {
                resolve(v)
            }, r => {
                reject(r)
            })
        } else {
            resolve(value)
        }
    })
}

// 添加reject方法
Promise.reject = function (value) {
    return new Promise((resolve, reject) => {
        reject(value)
    })
}

// 添加all方法
Promise.all = function (promises) {
    return new Promise((resolve, reject) => {
        //声明变量
        let cnt = 0;
        let arr = [];
        //遍历
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(v => {
                cnt++;
                arr[i] = v;
                if (cnt === promises.length) {
                    resolve(arr)
                }
            }, r => {
                reject(r)
            })
        }
    })
}

// 添加race方法
Promise.race = function (promises) {
    return new Promise((resolve, reject) => {
        for (let i = 0; i < promises.length; i++) {
            promises[i].then(v => {
                resolve(v)
            }, r => {
                reject(r)
            })
        }
    })
}

```



**index.html (测试html)**

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./promise.js"></script>
</head>
<body>
<script>
    let p = new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('OK')
        }, 1000)
        // resolve('OK')
        // reject('err')
        // throw 'error'
    })

    const res = p.then(value => {
        console.log(value)
    }, reason => {
        console.warn(reason)
    })

    // const res = p.catch(reason => {
    //     console.log('err')
    // })
    console.log(res)
    console.log(p)

</script>
</body>
</html>

```



手写学习教程：[BiliBili链接](https://www.bilibili.com/video/BV1GA411x7z1?p=18)