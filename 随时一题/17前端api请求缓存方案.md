## 前端 api 请求缓存方案

对于 webpack 打包的单页面应用程序，可以采用很多方式来进行性能优化，比如 **tree-shaking、模块懒加载、利用 extrens 网络 cdn 加速这些常规的优化。**甚至在 vue-cli 项目中我们可以用 --modern 指令生成新旧两份浏览器代码来对程序进行优化。

而事实上，缓存一定是提升 web 应用程序有效的方法之一，尤其是用户受限于网络的情况下。提升系统的响应能力，降低网络的消耗。当然，内容越接近于用户，则缓存的速度就会越快，缓存的有效性则会越高。

以客户端而言，我们有很多缓存数据与资源的方法，例如，**标准的浏览器缓存**，**Service Worker**，但是，它们更适合 html、css、js以及图片等静态内容的缓存。对于缓存系统数据，我们需要采用另外的方法，接下来对应用到项目中的各种 api 请求缓存方案，从简单到复杂依次介绍一下。

### 方案一：数据缓存

简单的数据缓存，第一次请求时获得数据，之后便使用数据，不再请求后端 api。代码如下：

```js
const dataCache = new Map()

async getWares() {
  let key = 'wares'
  // 从 data 缓存中获取数据
  let data = dataCache.get(key)
  if (!data) {
    // 没有数据请求服务器
    const res = await request.get('/getWares')
    
    // 其他操作
    ...
    data = ...
    
    // 设置数据缓存
    dataCache.set(key, data)
    
  }
  return data
}
```

第一行代码创建了一个 Map 对象，作为一个键值对存储结构。之后，使用 async 函数，用同步的方式写异步，使异步操作变得方便。

代码本身很容易理解，是利用 Map 对象对数据进行缓存，之后调用从 Map 对象来取数据。对于简单的业务场景，直接使用以上代码即可。

**调用方式：**

```js
getWares().then(...)
// 第二次调用，取得先前的data
getWares().then(...)
```

