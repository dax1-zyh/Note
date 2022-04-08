## 解析url中所有的参数名称与值
主要实现思路是将url中参数部分的字符串截取出来，并一步一步的地拆分，最终拆为一个个的键值对，存在对象中。

```javascript
    const getQuery = (url) => {
        // str为？之后的参数部分字符串
        const str = url.substr(url.indexOf('?') + 1)
        // arr每个元素都是完整的参数键值
        const arr = str.split('&')
        // result为存储参数键值的集合
        const result = {}
        for (let i = 0; i < arr.length; i++) {
            // item的两个元素分别为参数名和参数值
            const item = arr[i].split('=')
            result[item[0]] = item[1]
        }
        return result[query]
    }

    const res = getQuery('https://www.google.com/search?a=123&b=adbxo213&c=UTF-8')
    console.log(res)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/eb1a7805de9c4dab91d9a2a938a4583d.png)


## 解析url中指定的参数名称与值
与上方思路相同，只要传递和返回响应的参数就可以。

```javascript
    const getQuery = (url,query) => {
        // str为？之后的参数部分字符串
        const str = url.substr(url.indexOf('?') + 1)
        // arr每个元素都是完整的参数键值
        const arr = str.split('&')
        // result为存储参数键值的集合
        const result = {}
        for (let i = 0; i < arr.length; i++) {
            // item的两个元素分别为参数名和参数值
            const item = arr[i].split('=')
            result[item[0]] = item[1]
        }
        return result
    }

    const res = getQuery('https://www.google.com/search?a=123&b=adbxo213&c=UTF-8','a')
    console.log(res)
```