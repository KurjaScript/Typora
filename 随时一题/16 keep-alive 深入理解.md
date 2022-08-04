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

