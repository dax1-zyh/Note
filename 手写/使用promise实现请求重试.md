使用promise实现请求重试：若请求失败，则延迟n秒之后重新发送请求，有最大重试次数。

```javascript
// getData 请求函数
// times 最大重试次数
// delay 重试延迟时间
function retry(getData, times, delay) {
    return new Promise((resolve, reject) => {
        function attempt() {
            getData.then(resolve).catch((err) => {
                console.log(`还有${times}次机会`)
                if(times == 0) {
                    reject(err)
                } else {
                    times--
                    setTimeout(attempt(), delay)
                }
            })
        }
        attempt()
    })
}
```