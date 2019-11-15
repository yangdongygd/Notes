# axios的基础使用

## 简介

axios是一个基于 Promise 的 http 库，可以在浏览器和 node.js 中使用

## 功能

1. 创建 XMLHttpRequest 请求
2. 在 node.js 中创建 http 请求
3. 支持 Promise API
4. 配置请求和响应的拦截，对请求和响应近行预处理
5. 取消请求
6. 自动转换 JSON 数据
7. 客户端支持防御 XSRF 攻击

## 安装

使用npm:

```shell
$ npm install axios
```

使用yarn:

```shell
$ yarn add axios
```

## 使用

```javascript
import axios from 'axios'

// 直接使用
axios.get(url, {params: params}) // url是必须的，params参数是可选的
	.then(res => {
    	console.log(res) // 成功
    })
	.catch(err => {
    	console.log(err) // 失败
    })

axios.post(url, {...data}) // url是必须的，data参数是可选的
	.then(res => {
    	console.log(res) // 成功
    })
	.catch(err => {
    	console.log(err) // 失败
    })

// 配置相关参数使用
let request = axios.create({
    baseURL: _API_, // 每个请求前的基础地址
    timeout: 1000, // 超时时间
    headers: { // 请求头
        'X-Requested-With': 'XMLHttpRequest',
      	'Content-Type': 'application/json'
    },
    responseType: 'json', // 默认的请求类型，可选'arraybuffer', 'blob', 'document', 'json', 'text', 'stream'
    withCredentials: false, // 默认跨域请求时不需要使用凭证
})
```

## 响应结构

```javascript
{
  data: {}, // `data` 由服务器提供的响应

  status: 200, // `status` 来自服务器响应的 HTTP 状态码

  statusText: 'OK', // `statusText` 来自服务器响应的 HTTP 状态信息

  headers: {}, // `headers` 服务器响应的头

  config: {} // `config` 是为请求提供的配置信息
}
```

## 拦截器

```javascript
// 请求拦截器
axios.interceptors.request.use(config => {
    // 对请求参数进行处理
    return config
  }, error => {
    // 对请求错误进行处理
    return Promise.reject(error);
  });

// 响应拦截器
axios.interceptors.response.use(response => {
    // 对响应数据进行处理
    return response
  }, error => {
    // 对响应错误进行处理
    return Promise.reject(error);
  })
```



