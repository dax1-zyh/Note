async/await是Promise的语法糖

## async
- async作为一个关键字，放在函数前面，表示函数是一个异步函数。
- async函数**返回一个 Promise对象**，并且是resolve的结果，可以使用then方法添加回调函数。
- 只要使用async，不管函数内部返回的是不是Promise对象，都会被包装成Promise对象。

```javascript
// async基础语法
async function fun0(){
    console.log(1);
    return 1;
}
fun0().then(val=>{
    console.log(val) // 1,1
})

async function fun1(){
    console.log('Promise');
    return new Promise(function(resolve,reject){
        resolve('Promise')
    })
}
fun1().then(val => {
    console.log(val); // Promise Promise
})

```

## await
- await 也是一个修饰符，只能放在async定义的函数内。

- await 修饰的如果是Promise对象：可以获取Promise中返回的内容（resolve或reject的参数），且取到值后语句才会往下执行；

- 如果不是Promise对象：把这个非promise的东西当做await表达式的结果。

```javascript
async function fun(){
    let a = await 1;
    let b = await new Promise((resolve,reject)=>{
        setTimeout(function(){
            resolve('setTimeout')
        },3000)
    })
    let c = await function(){
        return 'function'
    }()
    console.log(a,b,c)
}
fun(); // 3秒后输出： 1 "setTimeout" "function"

```

```javascript
function log(time){
    setTimeout(function(){
        console.log(time);
        return 1;
    },time)
}
async function fun(){
    let a = await log(1000);
    let b = await log(3000);
    let c = log(2000);
    console.log(a);
    console.log(1)
}
fun(); 
// 立即输出 undefined 1
// 1秒后输出 1000
// 2秒后输出 2000
// 3秒后输出 3000

```

## async，await配合

```javascript
// 使用async/await获取成功的结果

// 定义一个异步函数，3秒后才能获取到值(类似操作数据库)
function getSomeThing(){
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{
            resolve('获取成功')
        },3000)
    })
}

async function test(){
    let a = await getSomeThing();
    console.log(a)
}
test(); // 3秒后输出：获取成功

```

## 异常处理
 await后面的函数是有可能出现异常的，所以最好把await命令放在try...catch代码块中。如果await后是Promise对象，也可以使用.catch进行捕获。

```javascript
 // 第一种写法
 async function myFunction() {
   try {
     await something();
   } catch (err) {
     console.log(err);
   }
 }
 
 // 第二种写法
 async function myFunction() {
   await somethingPromise()
   .catch(function (err) {
     console.log(err);
   });
 }

```