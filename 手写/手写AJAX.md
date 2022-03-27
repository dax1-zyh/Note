```javascript
    function ajax(method, url, success, fail) {
        let request = new XMLHttpRequest()
        request.open(method, url)
        request.onreadystatechange = () => {
            if (request.readyState === 4) {
                if (request.state >= 200 && request.state < 300 || request.state === 304) {
                    success(request)
                } else {
                    fail(request)
                }
            }
        }
        request.send()
    }
```