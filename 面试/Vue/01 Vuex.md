### 1. vuex 是什么？

`vuex` 是一个状态管理模式，相当于一个仓库，里面存放了很多对象。

### 2. 能说得再具体点么？(怎么使用vuex)

其中 state 是数据源存放地，对应于一般 vue 对象里的 `data`，`state` 里存放的数据是响应式的， 它通过 `mapState` 把全局的 `state` 和 `getters` 映射到当前组件的 `computed` 计算属性中。

`vue` 组件从 `store` 读取数据，若是 `store` 中的数据发生改变，依赖这个数据的组件也会自动更新。

### 3. 说一说 vuex 有哪些属性？

Vuex 有 5 种属性：分别是 state、getters、mutation、action、module；

- 其中 state 相当于 vue 组件中的 data；

- getters 相当于 vue 组件中的 computed；
- mutation 用于修改 store 里的状态或数据，通过辅助函数 mapMutation 把 store 里的方法引入组件；
- action 用来异步操作数据的，但我没有用过这个额属性。

