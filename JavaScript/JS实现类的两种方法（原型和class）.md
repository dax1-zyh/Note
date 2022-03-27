## 方法一：使用原型

```javascript
    function Dog (name) {
        this.name = name
        this.legsNumber = 4
    }
    Dog.prototype.say= function (){
        console.log(`汪汪汪,我是${this.name},我有${this.legsNumber}条腿`)
    }

    const dog1 = new Dog('贝拉')
    dog1.say()
```
将实例本身的属性写在构造函数内部，需要继承的共有属性写在原型对象上。

## 方法二：使用class

```javascript
    class Dog2 {
        constructor(name) {
            this.name = name
            this.legsNumber = 4
        }
        say() {
            console.log(`汪汪汪,我是${this.name},我有${this.legsNumber}条腿`)
        }
    }
    const dog2 = new Dog2('大黄')
    dog2.say()
```
将实例本身的属性写在`constructor`内部，需要继承的共有属性写在`class`内，`constructor`之外的部分