# new的背后原理

```javascript
function Fun(){}

let obj = Object.create(Fun.prototype)// 第一步，创建新对象并指向构造函数的原型
let result = Fun.call(obj)// 第二步，执行构造函数
if (typeof result === 'object') {// 第三步，如果生成的结果是一个对象，则返回这个对象
    return result
} else {// 否则，返回新创建的对象
    return obj
}
```

