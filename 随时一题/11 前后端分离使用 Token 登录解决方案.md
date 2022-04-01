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

### 定义路由

```js
const router = new Router({
    mode: 'history'
    routes: [
    	{
    		path: '/',
    		name: 'Index',
    		component: Index,
    		meta: {
    			requiresAuth: true
			}
		},
        {
        	path: '/login',
            name: 'Login',
            component: login
        }
    ]
})
```

上面我给首页路由加了 `requiresAuth`，所以使用路由钩子来拦截导航，`localStorage` 里有 `token` ，就调用获取 `userInfo` 的方法，并继续执行，如果没有 `token` ，调用退出登录的方法，重定向到登录页。

``` js
router.beforeEach((to,from,next) => {
    let token = window.localStorage.getItem('token')
    if (to.meta.requiresAuth) {
        if (token) {
            store.dispatch('getUser')
            next()
        } else {
            store.dispatch('logout')
            next({
                path: '/login',
                query: {redirect: to.fullPath}
            })
        }
    } else {
        next()
    }
})
```

这里使用了 `Vuex` 的 `action` 方法，马上就会说到。

### Vuex

首先，在 `mutation_types` 里定义：

```js
export const LOGIN = 'LOGIN' // 登录
export const USERINFO = 'USERINFO' // 用户信息
export const LOGINSTATUS = 'LOGINSTATUS' // 登录状态
```

然后在 `mutation` 里使用它们：

```js
const mutations = {
  [types.LOGIN]: (state, value) => {
    state.token = value
  },
  [types.USERINFO]: (state, info) => {
    state.userInfo = info
  },
  [types.LOGINSTATUS]: (state, bool) => {
    state.loginStatus = bool
  }
}
```

在之前封装 Axios 的 JS里定义请求接口：

```js
export const login = ({ loginUser, loginPassword }) => {
  return instance.post('/login', {
    username: loginUser,
    password: loginPassword
  })
}

export const getUserInfo = () => {
  return instance.get('/profile')
}
```

在 Vuex 的 actions 里引入：

```js
import * as types from './types'
import { instance, login, getUserInfo } from '../api'
```

定义 action：

```js
export default {
  toLogin ({ commit }, info) {
    return new Promise((resolve, reject) => {
      login(info).then(res => {
        if (res.status === 200) {
          commit(types.LOGIN, res.data.token) // 存储 token
          commit(types.LOGINSTATUS, true)     // 改变登录状态为 
          instance.defaults.headers.common['Authorization'] = `Bearer ` + res.data.token // 请求头添加 token
          window.localStorage.setItem('token', res.data.token)  // 存储进 localStorage
          resolve(res)
        }
      }).catch((error) => {
        console.log(error)
        reject(error)
      })
    })
  },
  getUser ({ commit }) {
    return new Promise((resolve, reject) => {
      getUserInfo().then(res => {
        if (res.status === 200) {
          commit(types.USERINFO, res.data) // 把 userInfo 存进 Vuex
        }
      }).catch((error) => {
        reject(error)
      })
    })
  },
  logOut ({ commit }) { // 退出登录
    return new Promise((resolve, reject) => {
      commit(types.USERINFO, null)        // 情况 userInfo
      commit(types.LOGINSTATUS, false)  // 登录状态改为 false
      commit(types.LOGIN, '')          // 清除 token
      window.localStorage.removeItem('token')
    })
  }
}
```

### 接口

这时候，该去写后端接口了。这里用了阿里的 egg 框架，感觉很强大。

首先定义一个 `LoginController` ：

```js
const Controller = require('egg').Controller;
const jwt = require('jsonwebtoken'); // 引入 jsonwebtoken

class LoginController extends Controller {
  async index() {
    const ctx = this.ctx;
/*
 把用户信息加密成 token ,因为没连接数据库，所以都是假数据
正常应该先判断用户名及密码是否正确
*/
    const token = jwt.sign({       
      user_id: 1,      // user_id
      user_name: ctx.request.body.username // user_name
    }, 'shenzhouhaotian', { // 秘钥
        expiresIn: '60s' // 过期时间
    });
    ctx.body = {             // 返回给前端
      token: token
    };
    ctx.status = 200;           // 状态码 200
  }
}

module.exports = LoginController;
```

然后定义一个 `UserController` ：

```js
class UserController extends Controller {
  async index() {
    const ctx = this.ctx
    const authorization = ctx.get('Authorization');
    if (authorization === '') { // 判断请求头有没有携带 token ,没有直接返回 401
        ctx.throw(401, 'no token detected in http header "Authorization"');
    }
    const token = authorization.split(' ')[1];
    // console.log(token)
    let tokenContent;
    try {
        tokenContent = await jwt.verify(token, 'shenzhouhaotian');     //如果 token 过期或验证失败，将返回401
        console.log(tokenContent)
        ctx.body = tokenContent     // token有效，返回 userInfo ;同理，其它接口在这里处理对应逻辑并返回
    } catch (err) {
        ctx.throw(401, 'invalid token');
    }
  }
}
```

在 `router.js` 里定义接口：

```js
module.exports = app => {
  const { router, controller } = app;
  router.get('/', controller.home.index);
  router.get('/profile', controller.user.index);
  router.post('/login', controller.login.index);
};
```

