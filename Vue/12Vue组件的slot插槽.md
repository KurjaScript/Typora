### 1. 为什么使用插槽

为让我们封装的组件具有更强的扩展性。我们可以在双闭合标签内再编写一些其他内容，把这些内容作为组件扩充内容。

### 2. 插槽的基本使用

- 在组件里定义一个`<slot></slot>`
- 插槽的默认值, 例如`<slot>button</slot>`
- 如果有多个值，同时放入到组件进行替换时，则一起作为替换元素

### 3. 具名插槽的使用

为插入多个值中的某一个

原组件

```html
<template id="cpn">
	<div>
		<slot name="left"><span>左边</slot>
		<slot name="center"><span>中间</slot>
		<slot name="right"><span>右边</slot>
	</div>
</template>
```

改变

```html
<div id="app">
	<cpn><span slot="center">标题</span></cpn>
	<cpn><button slot="left">返回</button></cpn>
</div>
```

### 4. 作用域插槽

在此之前，先弄明白什么是**编译的作用域**。

官方给出一条准则：**父组件模板的所有东西都会在父级组件作用域内编译，子组件模板的所有东西都会在子级作用域内编译。**

作用域插槽的一个目的就是：**父组件替换插槽的标签，但是内容由子组件来提供。**