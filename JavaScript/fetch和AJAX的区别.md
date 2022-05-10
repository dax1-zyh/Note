## AJAX

```javascript
$.ajax({
   type: 'POST',
   url: url,
   data: data,
   dataType: dataType,
   success: function () {},
   error: function () {}
});
```
传统AJAX指的是`XMLHttpRequest`，最早出现的发送后端请求的技术。


## fetch

```javascript
try {
  let response = await fetch(url);
  let data = response.json();
  console.log(data);
} catch(e) {
  console.log("Oops, error", e);
}
```

fetch是基于Promise设计的，**fetch不是AJAX的进一步封装，而是原生JS，没有使用XMLHttpRequest对象。**

## fetch和AJAX的主要区别
### 1.fetch返回错误的http状态码时不会reject
fetch只对网络请求报错，对400，500都当做成功的请求，服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。

**解决方案：**

我们可以在第一层.then里面去手动抛出异常，之后就可以使用.then .catch了。

```javascript
fetch(
        'http://domain/service', {
            method: 'GET'
        }
    )
    .then(response => {
        if (response.ok) {
            return response.json();
        }
        throw new Error('Network response was not ok.');
    })
    .then(json => console.log(json))
    .catch(error => console.error('error:', error));
```

### 2.默认情况下，fetch不会携带cookie

**解决方案：**

添加配置项： `fetch(url, {credentials: 'include'})`


### 3.fetch没有设置超时时间
**解决方案：**
使用一个promise来封装，通过setTimeout()来设置一个时间，如果超过该事件，就返回reject

```javascript
    return new Promise((resolve, reject) => {
        fetch(url, init)
            .then(resolve)
            .catch(reject);
        setTimeout(reject, timeout);
    })
}
```

在这里，fetch会和setTimeout同时执行，因为fetch是异步的，不会堵塞后面setTimeout的执行