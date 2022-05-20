```javascript

let arr = [1,1,"1","1",true,true,"true",{},{},"{}",null,null,undefined,undefined]

// 方法1
let uniqueOne = Array.from(new Set(arr)) console.log(uniqueOne)

// 方法2
let uniqueTwo = arr => {
    let map = new Map(); //或者用空对象 let obj = {} 利用对象属性不能重复得特性
    let brr = []
    arr.forEach( item => {
        if(!map.has(item)) { //如果是对象得话就判断 !obj[item]
            map.set(item,true) //如果是对象得话就obj[item] =true 其他一样
            brr.push(item)
        }
    })
    return brr
}
console.log(uniqueTwo(arr))

//方法3
let uniqueThree = arr => {
    let brr = []
    arr.forEach(item => {
        // 使用indexOf 返回数组是否包含某个值 没有就返回-1 有就返回下标
        if(brr.indexOf(item) === -1) brr.push(item)
        // 或者使用includes 返回数组是否包含某个值 没有就返回false 有就返回true
        if(!brr.includes(item)) brr.push(item)
    })
    return brr
}
console.log(uniqueThree(arr))

//方法4
let uniqueFour = arr => {                                         
     // 使用 filter 返回符合条件的集合
    let brr = arr.filter((item,index) => {
        return arr.indexOf(item) === index
    })
    return brr
}
console.log(uniqueFour(arr))


```

