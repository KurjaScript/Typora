先看一段代码

```html
<script setup lang="ts">
import {reactive} from 'vue'
let message = reactive<number[]>([])
setTimeout(() => {
  let arr = [1, 2, 3, 4]
  message = arr // 这种直接赋值的方式会破坏 message 的响应式
	console.log(message) // 输出为空，并没有更新
}, 1000)
</script>
```

可以通过 push，但如果是想修改里面值，可以通过以下方式

```html
<script setup lang="ts">
import {reactive} from 'vue'
type O = {
	list: number[]
}
let message = reactive<O>({
    list: []
  })
setTimeout(() => {
  message.list = [1, 2, 3, 4, 5]
}, 1000)
</script>
```

readonly: 拷贝一份 proxy 对象将其设置为只读

```
import {reactive, readonly} from 'vue'
const person = reactive({count:only})
const copy = readonly(person)

copy.count++
```

