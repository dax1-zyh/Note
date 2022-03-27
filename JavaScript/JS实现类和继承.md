## 实现类
### 方法一：使用原型

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

### 方法二：使用class

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


## 实现继承

假设`Dog`需要从`Animal`身上继承`legsNumber`的属性

### 方法一：使用原型

```javascript
    function Animal (legsNumber) {
        this.legsNumber = legsNumber
    }

    function Dog (name) {
        this.name = name
        Animal.call(this,4) // 关键代码1
    }

    Dog.prototype.__proto__ = Animal.prototype // 关键代码2

    Dog.prototype.say= function (){
        console.log(`汪汪汪,我是${this.name},我有${this.legsNumber}条腿`)
    }

    const dog1 = new Dog('贝拉')
    dog1.say()
```

上方，关键代码2在某些浏览器可能是无效的，需要用这三行替代

```javascript
    var f = function(){ }
    f.prototype = Animal.prototype
    Dog.prototype = new f()
```

### 方法二：使用class

```javascript
    class Animal2 {
        constructor(legsNumber) {
            this.legsNumber = legsNumber
        }
    }

    class Dog2 extends Animal2 {    // 关键代码1
        constructor(name) {
            super(3)    // 关键代码2
            this.name = name
        }
        say() {
            console.log(`汪汪汪,我是${this.name},我有${this.legsNumber}条腿`)
        }
    }
    const dog2 = new Dog2('大黄')
    dog2.say()
```