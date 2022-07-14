watch是一个函数,第一个参数是侦听的对象，第二个参数是一个关于新旧值的函数，第三个参数是{deep: true, immediate: true}。



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