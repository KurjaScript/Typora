### watch

watch是一个函数,第一个参数是侦听的对象，第二个参数是一个关于新旧值的函数，第三个参数是{deep: true, immediate: true}。

如果想一开始就在回调函数中做一些动作，那就在第三个参数中使用 `immediate: true`

```
import { ref, watch } from 'vue'
let message = ref({
	nav: {
		bar: {
			name: 'Kurja'
		} 
	}
})
watch(message, (newVal, oldVal) => {
	console.log('新的', newVal)
	console.log('旧的', oldVal)
}, {
	deep: true,
	immediate: true
})
```

如果是用 reactive，即使不写第三个参数，也是可以侦听到对象的深层次的属性。

如果只想侦听对象中的某一个属性值，可以把第一个参数传入一个返回改属性的函数。`watch(() => message.name,(newVal, oldVal) => {...})`

### watchEffect

#### flush

首先看一段代码

```tsx
const childOne = ref(null)
watchEffect(() => {
  console.log('打印', childOne.value)
}, {
  flush: 'post'
})
```

![](/Users/Kurja/Desktop/Typora/Vue/Vue%203/%E7%AC%AC%E4%BA%8C%E6%AC%A1%E7%B3%BB%E7%BB%9F%E5%AD%A6%E4%B9%A0/e6c9d24egy1h46ji24aghj204g01amwx.jpg)

如果不加 flush 属性，就会打印模版还会挂载的值，加了 flush 一定是挂载完毕才会打印。

#### stop

在写 vue2.0 时，有时候想要停止监听却无计可施，只能眼巴巴等到组件被销毁才能停止。vue3 提供了 stop 函数来停止监听。

停止监听，比较简单的一个方式就是再调一次 watch() 函数的返回值。如下：

```
const stop = watch(count, (val,oldVal) => {
	if(val !== oldVal) {
		data.value.pageNum = val
		// 当页码不一样调取接口
	}
})
// 然后在某个函数中调用 stop() 函数，监听就停止了
```

