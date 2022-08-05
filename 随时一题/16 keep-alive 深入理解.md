## keep-alive 深入理解及实践总结

### 1. 什么是 keep-alive

在平常的开发中，有部分组件没有必要多次初始化，这时，我们需要将组件持久化，**使组件的状态维持不变**，在下一次展示时，也不会进行重新初始化组件。

`<keep-alive>` 是 vue 内置的一个组件，在组件切换的过程中，可以**使被包含组件的状态保留在内存中，避免重新渲染 DOM。**也就是所谓的**组件缓存**。

> <keep-alive> 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。和 `<transition>` 相似， `<keep-alive> ` 是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中。

**prop：**

- include：字符串或正则表达式。只有匹配的组件会被缓存。
- exclude：字符串或正则表达式。任何匹配的组件都不会被缓存。

### 2. keep-alive 的生命周期执行

- 页面第一次进入，钩子的触发顺序：`created -> mounted -> activated`，退出时触发 `deactivated`，当再次进入（前进或者后退）时，只触发 `activated`
- 事件挂载的方法等，只执行一次放在 mounted 中；组件每次进去执行的方法在 `activated` 中。

> 当组件在 `<keep-alive>` 内被切换，它的 activated 和 deactivated 这两个生命周期钩子函数会被对应执行。

如果`<keep-alive>` 包裹两个组件：组件 A 和组件 B 。当第一次切换组件 A 时，组件 A 的 created 和 activated 生命周期函数会执行，这时通过点击事件改变组件 A 的文字的颜色，再切换组件 B，这时组件 A 的 deactivated 的生命周期函数会被触发；再切换回组件 A，组件 A 的 activated 生命周期函数会被触发，但是它的 created 生命周期函数不会被触发了，而且 A 组件的文字的颜色也是之前我们设置过的。

### 3. 基本用法

```html
<!--被keep-alive包含的组件会被缓存-->
<keep-alive>
    <component><component />
</keep-alive>
```

被`keepalive`包含的组件不会被再次初始化，也就意味着**不会重走生命周期函数**。

但有时我们希望缓存的组件也能够再次被渲染，vue 也为我们解决了这个问题。创建在 `keep-alive` 中的组件，会多出两个生命周期的钩子：`activated` 与  `deactivated`。

- `activated`：当 `keep-alive` 包含的组件再次渲染的时候触发；
- `deactivated `：当 `keep-alive` 包含的组件被销毁的时候触发。

keep-alive 缓存的组件不会被 mounted，为此提供`activated` 和 `deactivated` 钩子函数。

### 4. 参数理解

`keep- alive` 可以接收三个属性作为参数匹配对应的组件进行缓存。

- `include` 包含的组件(可以为字符串，数组，以及正则表达式,只有匹配的组件会被缓存)

- `exclude` 排除的组件(以为字符串，数组，以及正则表达式,任何匹配的组件都不会被缓存)

- `max` 缓存组件的最大值(类型为字符或者数字,可以控制缓存组件的个数)

**注：**当使用**正则表达式**或者**数组**时，一定要使用 v-bind。

```html
<!-- 将（只）缓存组件name为a或者b的组件, 结合动态组件使用 -->
<keep-alive include="a,b">
  <component></component>
</keep-alive>

<!-- 组件name为c的组件不缓存(可以保留它的状态或避免重新渲染) -->
<keep-alive exclude="c"> 
  <component></component>
</keep-alive>

<!-- 使用正则表达式，需使用v-bind -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- 动态判断 -->
<keep-alive :include="includedComponents">
  <router-view></router-view>
</keep-alive>

<!-- 如果同时使用include,exclude,那么exclude优先于include， 下面的例子只缓存a组件 -->
<keep-alive include="a,b" exclude="b"> 
  <component></component>
</keep-alive>

<!-- 如果缓存的组件超过了max设定的值5，那么将删除第一个缓存的组件 -->
<keep-alive exclude="c" max="5"> 
  <component></component>
</keep-alive>
```

### 5. 遇见 `vue-router` 结合 `router` 使用，缓存部分页面

#### 5.1 所有路径下的视图组件都会被缓存

```html
<keep-alive>
	<router-view>
  	<!-- 所有路径匹配到的视图组件都会被缓存 -->
  </router-view>
</keep-alive>
```

#### 5.2 缓存 `router-view` 里面某个组件

- 使用 `include/exclude`

  > 缺点：需要知道组件的名字，项目复杂的时候，不是一个很好的选择

  ```html
  <keep-alive include="a">
  	<router-view>
    	<!-- 只有路径匹配到的 include 为 a 组件会被缓存 -->
    </router-view>
  </keep-alive>
  ```

- 使用meta 属性

  > 优点：不需要列举出需要被缓存组件的名字，使用 `$route.meta` 的 keepAlive 属性

  ```html
  <keep-alive>
  	<router-view v-if="$route.meta.keepAlive"></router-view>
  </keep-alive>
  <router-view v-if="!$route.meta.keeoAlive"></router-view>
  ```

  需要在 `router` 中设置 router 的元信息 meta：

  ```js
  // ...router.js
  export default new Router ({
    routes: [
      {
        path:'/',
        name: 'Hello',
        component: Hello,
        meta: {
          keepAlive: false // 不需要缓存
        }
      },
      {
        path: '/page1',
        name: 'Page1',
        component: Page1,
        meta: {
          keepAlive: true // 需要被缓存
        }
      }
    ]
  })
  ```

**【加盐】 使用 router.meta 拓展**

假设这里有三个路由：A、B、C。

- 需求：

  1. 默认显示 A；
  2. B 跳转到 A，A 不刷新；
  3. C 挑战到 A，A 刷新。

- 实现步骤：

  1. 在 A 路由里面设置 meta 属性：

     ```js
     {
       path: '/',
       name: 'A',
       component: A,
       meta: {
       	keepAlive: true // 需要被缓存
       }
     }
     ```

  2. 在 B 组件里面设置 beforeRouteLeave:

     ```js
     export default {
       data() {
         return {};
       }
       methods: {},
       beforeRouteLeave(to, from, next) {
     		// 设置下一个路由的 meta
       	to.meta.keepAlive = true; // 让 A 缓存，即不刷新
       	next()
     	}
     }
     ```

  3. 在 C 组件里面设置 beforeRouteLeave：

     ```js
     export default {
       data() {
         return {};
       },
       methods: {},
       beforeRouteLeave(to, from, next) {
         // 设置下一个路由的 meta
         to.meta.keepAlive = false; // 让 A 不缓存，即刷新
       }
     }
     ```

  这样，便实现 B 回到 A，A 不刷新；而 C 回到 A，A 刷新。

