# JS变量提升与函数提升

## 定义

在作用域中，无论是变量还是函数，都会提升到作用域最开始的位置，并且函数提升的位置会在变量提升之后。

## 例子

```javascript
function fn() {
    console.log(a)
    var a = 1
    console.log(a)
    function a() {}
    console.log(a)
    console.log(b)
    var b = 2
    console.log(b)
    function b() {}
    console.log(b)
}

fn()
```

## JS解析

```javascript
function fn() {
  var a
  var b
  function a() {}
  function b() {}
  console.log(a) // 输出 a()
  a = 1
  console.log(a) // 输出 1
  console.log(a) // 输出 1
  console.log(b) // 输出 b()
  b = 2;
  console.log(b) // 输出 2
  console.log(b) // 输出 2
}

fn()
```

## 隐式全局变量不会进行变量提升

```javascript
function fn() {
    console.log(a) // 报错：a is not defined
    a = 'hello world'
}

fn()
```

