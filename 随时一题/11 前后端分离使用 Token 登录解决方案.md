### 首先捋一下思路

- 首次登录时，后端服务器接收用户的账号和密码，验证成功后，个根据用户的 `id`、用户名、定义好的密钥以及过期时间生成 `token`， 返回给前端；
- 前端拿到 `token` 后，把它储存到 `localStorage` 和 `Vuex` 里；
- 前端的每一次路由跳转都要判断 `localStorage` 有没有 `token`，没有 `token` 则跳转到登录页，有的话就请求获取用户信息，改变登录状态；
- 每次请求接口，在 `Axios` 请求头里携带 `token`；
- 后端接口判断请求是否有 `token`，没有 `token` 或者 `token` 已过期，返回 `401` 状态码；
- 前端得到 `401`状态吗，重定向到登录页面。

### 首先，封装 Axios

把 `token` 存在 `localStorage`，检查有无 `Token` ，每次请求在 `Axios` 请求头上进行携带

```js
if (window.localStorage.getItem('token')) {
	Axios.defaults.headers.common['Authorization'] = `Bearer ` + window.localStorage.getItem('token')
}
```

使用 response 拦截器，对 2xx 状态码以外的结果进行拦截；

```js
instance.interceptors.response.use(response => {
    return response;
}, error => {
    if (error.response) {
        switch (error.response.status) {
            case: 401:
                router.replace({
                    path: 'login',
                    query: {redirect: router.currentRoute.fullPath} // 将跳转的路由 path 当作参数，登录成功后跳转到该路由
                })
        }
    }
    return Promise.reject(error.response)
})
```

