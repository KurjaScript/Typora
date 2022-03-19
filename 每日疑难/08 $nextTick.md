### 1. 事件循环说明

简单来说，Vue 在修改数据后，视图不会立刻更新，而是等**同一事件循环**中的所有数据变化完成之后，再统一进行视图更新。

知乎上的例子：

```js
// 改变数据
vm.message = 'changed'

// 想要立即使用更新后的DOM，这样不行，因为设置message后DOM还没更新
console.log(vm.$el.textContent) // 并不会得到'changed'

// 这样可以，nextTick 里面的代码会在DOM更新后执行
Vue.nextTick(function(){
    console.log(vm.$el.textContent) // 可以得到'changed'
})
```

### 2. 用途

**应用场景**：需要在视图更新之后，基于新的视图进行操作。

#### 2.1 created、mounted

需要注意的是，在created和mounted阶段，如果需要操作渲染后的视图，也要使用nextTick方法。

官方文档说明：

mounted不会承诺所有子组件也都一起被挂载，如果你希望等到整个视图都渲染完毕，可以用`vm.$nextTick`替换掉`mounted`

```js
// mounted() {}
mounted: function () {
    this.$nextTick(function (){
        // Code that will run only after the entire view has been rendered
    })
}
```

#### 2.2 其他应用场景

其他应用场景如下三例

##### 2.2.1 点击按钮显示原本以`v-show = false`隐藏起来的输入框，并获取焦点

```js
showsou() {
	this.showit = true // 修改 v-show
    document.getElementById("keywords").focus() // 在第一个 tick 里，获取不到输入框，自然也获取不到焦点
}
```

修改为：

```js
showou() {
    this.showit = true;
    this.$nextTick(function () {
        // DOM更新了
        document.getElementById("keywords").focus()
    })
}
```

##### 2.2.2 点击获取元素宽度

```vue
<div id = "app">
    <p ref="myWidth" v-if="showMe">{{mesage}}</p>
    <button @click = "getMyWidth">获取p元素宽度</button>
</div>

getMyWidth() {
	this.showMe = true;
	// this.message = this.$refs.myWidth.offsetWidth;
	// 报错 TypeError: this.$refs.myWidth is undefined
	this.$nextTick(() =>{
		// DOM元素更新后执行，此时能拿到p元素的属性
		this.message = this.$refs.myWidth.offsetWidth;
	})
}
```

##### 2.2.3 使用 swiper 插件通过ajax请求图片后的滑动问题

