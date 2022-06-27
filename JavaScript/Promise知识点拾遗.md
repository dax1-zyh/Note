## 前言

本文大部分内容来自阮一峰所著ES6入门教程，原文链接在文尾。

本文权为个人整理的简洁版。

## 什么是Promise

        Promise是一个对象，从它可以获取异步操作的消息。 Promise提供统一的API，各种异步操作都可以用同样的方法进行处理。

## Promise的特点

- **对象的状态不受外界影响。** Promise有三种状态，`pending`（进行中）、`resolved`（已完成，又称fulfilled）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

- **一旦状态改变，就不会再变，任何时候都可以得到这个结果。** Promise对象的状态只可能有两种改变可能：从`pending`变为`resolved`或者从`pending`变为`rejected`。只要这两种情况发生，状态就不会再变了。

## Promise解决的问题

- Promise可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数，俗称`回调地狱`
- Promise提供统一的接口，使得异步操作更加容易。

## Promise的缺点

- 无法取消Promise，一旦新建它就会立即执行，无法中途取消
- 如果不设置回调函数，Promise内部抛出的错误，不会反映到外部
- 当处于`pending`状态时，无法得知目前进展到哪一状态

## 基本用法

Promise对象是一个构造函数，用来生成Promise实例。

下面代码创造了一个Promise实例

```javascript
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

Promise接受一个函数作为参数，该参数的两个参数分别为`resoleve`和`reject`，他们是两个函数。

`resolve`函数的作用是，将Promise状态从`pending`变为`resolved`，在异步操作成功时调用，并将异步操作的结果，作为参数传递出去。

`reject`函数的作用是，将Promise状态从`pending`变为`reject`，在异步操作失败时调用，并将异步操作报出的错误作为参数传递出去。

Promise实例生成以后，可以通过`then`方法分别指定`resolved`状态和`reject`状态的回调函数。

```javascript
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

`then`方法可以接受两个回调函数作为参数。第一个回调函数是Promise对象的状态变为Resolved时调用，第二个回调函数是Promise对象的状态变为Reject时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受Promise对象传出的值作为参数。

## then方法

`then`方法是定义在原型对象Promise.prototype上的，它的作用是为Promise实例添加状态改变时的回调函数。

`then`方法返回的是一个新的Promise实例，因此可以采用链式写法，即`then`方法后面再调用另一个`then`方法。

```javascript
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

上面的代码使用`then`方法，依次指定了两个回调函数。第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。

## catch方法

`catch`方法实际上是`.then(null,rejection)`的别名，用于指定发生错误时的回调函数。

```javascript
getJSON("/posts.json").then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

如果Promise状态已经变成`Resolved`，再抛出错误是无效的。

```javascript
var promise = new Promise(function(resolve, reject) {
  resolve('ok');
  throw new Error('test');
});
promise
  .then(function(value) { console.log(value) })
  .catch(function(error) { console.log(error) });
// ok
```

Promise对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获。

一般来说，不要在`then`方法里面定义`Reject`状态的回调函数（即`then`的第二个参数），总是使用`catch`方法。

```javascript
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```

上面代码中，第二种写法要好于第一种写法，理由是第二种写法可以捕获前面`then`方法执行中的错误，也更接近同步的写法`（try/catch）`。

跟传统的`try/catch`代码块不同的是，如果没有使用`catch`方法指定错误处理的回调函数，Promise对象抛出的错误不会传递到外层代码，即不会有任何反应。

需要注意的是，`catch`方法返回的还是一个Promise对象，因此后面还可以接着调用`then`方法。

```javascript
var someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};

someAsyncThing()
.catch(function(error) {
  console.log('oh no', error);
})
.then(function() {
  console.log('carry on');
});
// oh no [ReferenceError: x is not defined]
// carry on
```

## all方法

`all`方法用于将多个Promise实例，包装成一个新的Promise实例。

```javascript
var p = Promise.all([p1, p2, p3]);
```

上面代码中，`Promise.all`方法接受一个数组作为参数，p1、p2、p3都是Promise对象的实例，如果不是，就会先调用下面讲到的`Promise.resolve`方法，将参数转为Promise实例，再进一步处理。

p的状态由p1、p2、p3决定，分成两种情况。

- 只有p1、p2、p3的状态都变成`fulfilled`，p的状态才会变成`fulfilled`，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

-只要p1、p2、p3之中有一个被`rejected`，p的状态就变成`rejected`，此时第一个被reject的实例的返回值，会传递给p的回调函数。

## race方法

`race`方法同样是将多个Promise实例，包装成一个新的Promise实例

```javascript
var p = Promise.race([p1, p2, p3]);
```

上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

Promise.race方法的参数与Promise.all方法一样，如果不是 Promise 实例，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。

下面是一个例子，如果指定时间内没有获得结果，就将Promise的状态变为reject，否则变为resolve。

```javascript
var p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
])
p.then(response => console.log(response))
p.catch(error => console.log(error))
```

上面代码中，如果5秒之内fetch方法无法返回结果，变量p的状态就会变为`rejected`，从而触发`catch`方法指定的回调函数。

## resolve方法

有时需要将现有对象转为Promise对象，`resolve`方法就起到了这个作用。

`resolve`等价于下面的写法

```javascript
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

## reject方法

`reject`方法会返回一个新的Promise实例，该实例的状态为`rejected`。它的参数用法与`resolve`方法完全一致。

```javascript
var p = Promise.reject('出错了');
// 等同于
var p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s){
  console.log(s)
});
// 出错了
```

## done方法

Promise对象的回调链，不管以`then`方法或`catch`方法结尾，要是最后一个方法抛出错误，都有可能无法捕捉到（因为Promise内部的错误不会冒泡到全局）。因此，我们可以提供一个`done`方法，总是处于回调链的尾端，保证抛出任何可能出现的错误。

```javascript
asyncFunc()
  .then(f1)
  .catch(r1)
  .then(f2)
  .done();
```

它的实现代码相当简单

```javascript
Promise.prototype.done = function (onFulfilled, onRejected) {
  this.then(onFulfilled, onRejected)
    .catch(function (reason) {
      // 抛出一个全局错误
      setTimeout(() => { throw reason }, 0);
    });
};
```

从上面代码可见，`done`方法的使用，可以像`then`方法那样用，提供`Fulfilled`和`Rejected`状态的回调函数，也可以不提供任何参数。但不管怎样，`done`都会捕捉到任何可能出现的错误，并向全局抛出。

## finally方法

`finally`方法用于指定不管Promise对象最后状态如何，都会执行的操作。它与`done`方法的最大区别，它接受一个普通的回调函数作为参数，不论状态，该函数不管怎样都必须执行。

下面是一个例子，服务器使用Promise处理请求，然后使用`finally`方法关掉服务器

```javascript
server.listen(0)
  .then(function () {
    // run test
  })
  .finally(server.stop);
```

它的实现也很简单。

```javascript
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```

## 应用：加载图片

我们可以将图片的加载写成一个Promise，一旦加载完成，Promise的状态就发生变化。

```javascript
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    const image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```

## 参考文档

[Promise - 阮一峰](https://es6.ruanyifeng.com/#docs/promise)
