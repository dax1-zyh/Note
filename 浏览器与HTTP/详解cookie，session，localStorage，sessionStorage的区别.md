## cookie 和 session 区别

 - cookie数据存放在**浏览器**中，session数据存放在**服务器**上
 - cookie是不安全的，别人可以分析存放在本地的cookie并进行cookie诈骗，考虑到**安全性能**，应尽量使用session
 - session会在一定时间内保存在服务器上。当访问增多时，会比较占用服务器的性能。考虑到**服务性能**，应尽量使用cookie
 - 单个cookie保存的数据不能超过4k，很多浏览器都限制一个站点最多保存20个cookie

cookie和session都用来存储用户信息，cookie存放于客户端有可能被窃取，所以cookie一般用来存放不敏感的信息，比如用户设置的**网站主题**，敏感的信息用session存储，比如用户的**登录信息**



## cookie，sessionStorage，localStorage 区别
 - HTML5中提出了**webStorage**的概念，webStorage包括sessionStorage和localStorage，**只为了保存数据，不会与服务器进行通信**

 - cookie,localStorage,sessionStorage都是在**客户端**（本地）保存数据，存储数据的类型：字符串

 - cookie会随着HTTP请求发送到服务器，webStorage不会随着HTTP发送到服务器端，所以**安全性**相对来说比cookie高，不必担心截获

 - webStorage拥有更大的存储量，cookie大小限制4kb，webStorage达到**5M或更大**

 - webStorage拥有`setItem,getItem,removeItem,clear`等api，cookie需要自己封装

 - **生命周期不同**（见后文），localStorage要手动清除，sessionStorage在浏览器关闭后清除

   

## 生命周期

 - cookie ：可设置失效时间，否则默认为关闭浏览器后消失

 - localStorage ：除非被手动清除，否则永久保存

 - sessionStorage：仅在当前网页会话下有效，关闭页面或浏览器后就会被清除

   

 ## API

 localStorage和sessionStorageAPI是类似的

```javascript
xxxxxStorage.setItem('key',value)
// 接收一个键和值作为参数，把键值对添加到存储中
// 如果键名存在，则更新其对应的值。

xxxxxStorage.getItem('key')
// 接收一个键名作为参数，返回键名对应的值

xxxxxStorage.removeItem('key')
// 接收一个键名作为参数，并把该键名从存储中删除

xxxxxStorage.clear()
// 清空存储中的所有数据
```