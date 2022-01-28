### 概述

Axios是一个基于promise的HTTP库

**本质是XMLHttpRequests请求，即ajax请求**

#### 特性

- 从浏览器中创建`XMLHttpRequests`；
- 从`node.js`创建`http`请求；
- 支持Promise API；
- 拦截请求和响应；
- 转换请求数据和响应数据；
- 取消请求；
- 自动转换JSON数据；
- 客户端支持防御XSRF

### 基本使用

Axios提供两种不同的形式来发送HTTP请求，一种是通过axios()方法，另一种是分别通过axios对象提供的与HTTP方法对应的方法来发送请求，如：

- axios.get(url)
- axios.post(url, data)
- axios.update(url)
- axios.update(url)
- axios.put(url,data)

axios.get()等方法在官方文档中被描述为请求方法的别名

#### axios API的使用

axios(config)方法接收一个对象，这个对象包含了一些对请求的配置，axios会根据这些配置来发送对应的请求。

最基本的配置项应该包括：

- method请求的方法(可选值：get, post等)
- url请求的地址(必须项)
- data请求发送的数据(post等请求需要)

默认的请求方法是get，所以如果是get请求可以不设置method

```js
axios({
    method: 'post',
    url: '/user/12345',
    data: {
        fisrtName: '云柱',
        lastName: '火柱'
    }
});
```

请求响应的处理在then和catch回调中，请求正常会进入then，请求异常则会进入catch。

```js
// 发送POST请求
axios({
    method: 'post',
    url: '/user/12345',
    data: {
        firstName: '云柱',
        lastName: '火柱',
    }
}).then(res => {
    console.log(res)
}).catch(err => {
    console.log(err)
})
```

```js
// 发送请求
axios('/url/12345');
```

个人观点：对于Axios，只需掌握axios() api的基本使用就可以了

