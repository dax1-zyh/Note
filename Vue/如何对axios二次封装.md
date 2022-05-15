## 步骤
- 引入axios
- 创建axios实例
- 设置请求拦截器（request interceptors）
- 设置响应拦截器（response interceptors）
- 暴露出去

## 具体代码
```javascript
//1. 先引入axios依赖包
import axios from "axios";
 
//2. axios创建对象
const Server = axios.create({
    baseURL: "", //管理后台要使用的接口的基地址
    withCredentials: true, //请求跨域时携带cookie
    timeout: 8000, //超时时间
    headers: {
        // 'Content-Type': 'application/x-www-form-urlencoded',
        // 'custome-header':'tianliangjiaoyu'
    }
})
 
//3. 定义前置拦截器，请求拦截器，请求发送出去之前触发的
Server.interceptors.request.use((config) => {
    //config 接口请求的配置信息
    return config;
}, (error) => {
    //报错的是时候抛出一个报错的信息
    return Promise.reject(error);
})
 
//4. 定义后置拦截器,响应拦截器, 服务器响应回来数据之前触发，
Server.interceptors.response.use((response) => {
    //响应回来的数据操作
    return response.data;
}, (error) => {
    //报错的是时候抛出一个报错的信息
    return Promise.reject(error);
})
 
//5. 暴露出去
export default Server;
```