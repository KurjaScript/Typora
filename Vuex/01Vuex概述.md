#### 1. 组件之间共享数据的方式

父向子传值：`v-bind`事件绑定

子向父传值：`v-on`事件绑定

兄弟组件之间共享数据：`EventBus`

- `$on`接收数据的那个组件
- `$emit`发送数据的那个组件

<font color='green'>如果我们需要大范围、频繁地实现组件之间地数据的共享，以上三种方案就有点力不从心了。</font>于是就引出了Vuex。

#### 2.  Vuex是什么

Vuex是实现组件全局状态（数据）管理的一种机制，可以方便的实现组件之间数据的共享。

![image-20220104143402038](C:\Users\Kurja\AppData\Roaming\Typora\typora-user-images\image-20220104143402038.png)

<font color='green'>存储在</font>`vuex`<font color='green'>中的数据都是响应式的，能够实现数据与页面的同步。</font>

对于私有数据，存储在组件自身的`data`即可；一般情况下，只有组件之间共享的数据，才有必要`vuex`中。

