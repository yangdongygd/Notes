# 原型和继承

## 原型链继承

​		原型链是实现继承的主要方法，其基本思想是利用原型让一个对象继承另一个对象的属性和方法。

​		构造函数、原型和实例的关系：每 个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型 对象的内部指针。那么，假如我们让原型对象等于另一个类型的实例，结果会怎么样呢？显然，此时的 原型对象将包含一个指向另一个原型的指针，相应地，另一个原型中也包含着一个指向另一个构造函数 的指针。假如另一个原型又是另一个类型的实例，那么上述关系依然成立，如此层层递进，就构成了实例与原型的链条，即原型链。 

```javascript
function Super() {
    this.games = ['魂斗罗', '超级玛丽']
}

function Sub() {}

// 继承Super
Sub.prototype = new Super()


let instance1 = new Sub()
instance1.games.push('消消乐')
console.log(instance1.games) // ['魂斗罗', '超级玛丽', '消消乐']

let instance2 = new Sub()
console.log(instance2.games) // ['魂斗罗', '超级玛丽', '消消乐']
```

​		缺点：共享原型中的属性；不能向父类构造函数中传递参数

## 构造函数继承

```javascript
function Super(name) {
    this.name = name
}

function Sub(name) {
    // 继承Super
    Super.call(this, name)
}

let instance1 = new Sub('Jack')
console.log(instance1.name) // 'Jack'

let instance2 = new Sub('Rose')
console.log(instance2.name) // 'Rose'
```

​		缺点：无法复用函数

## 组合继承(最常用)

```javascript
function Super(name) {
    this.name = name
    this.games = ['魂斗罗', '超级玛丽']
}

Super.prototype.getName = function() {
    return this.name
}

function Sub(name, age) {
    // 继承属性
    Super.call(this, name)
    this.age = age
}

// 继承方法
Sub.prototype = new Super()
Sub.prototype.constructor = Sub
Sub.prototype.getAge = function() {
    return this.age
}

let instance1 = new Sub('Jack', 28)
instance1.games.push('消消乐')
console.log(instance1.games) // ['魂斗罗', '超级玛丽', '消消乐']
console.log(instance1.getName()) // 'Jack'
console.log(instance1.getAge()) // 28

let instance2 = new Sub('Rose', 26)
console.log(instance2.games) // ['魂斗罗', '超级玛丽']
console.log(instance2.getName()) // 'Rose'
console.log(instance2.getAge()) // 26
```

​		缺点：调用两次父类构造函数

## 原型式继承

```javascript
function create(obj) {
    // 借用临时函数
    function Temp() {}
    Temp.prototype = obj
    return new Temp()
}
```

## 寄生式继承

```javascript
function create(obj) {
    // 借用临时函数
    function Temp() {}
    Temp.prototype = obj
    return new Temp()
}

function createAnother(obj) {
    let clone = create(obj)
    clone.getName = function() { // 给对象添加方法
        return 'Jack'
    }
    return clone
}
```

## 寄生组合式继承(最理想)

```javascript
function inherit(sub, sup) {
    let prototype = create(sup.prototype)
    prototype.constructor = sub
    sub.prototype = prototype
}

function Super(name) {
    this.name = name
}

Super.prototype.getName = function() {
    return this.name
}

function Sub(name, age) {
    Super.call(this, name)
    this.age = age
}

inherit(Sub, Super)

Sub.prototype.getAge = function() {
    return this.age
}
```



