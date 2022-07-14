如果使用 ref 函数将某个对象中的属性变成响应式数据，修改响应式数据是不会影响到原始数据的。

```tsx
import { ref } from 'vue';
export default {
  name: 'App',
  setup() {
    let obj = {name: 'Kurja', age: '26'};
    let newObj = ref(obj.name);
    function change() {
      newObj.value = '云柱';
      console.log(obj, newObj)
    }
    return {
      newObj,
      change
    }
  }
}
```

上述代码，当 change 执行的时候，响应式数据发生改变，而原始数据并不会改变。

**原因在于， ref 本质是拷贝，与原始数据没有引用关系。**

需要注意的是， `ref(obj.name)` 相当于 `ref('Kurja')` 相当于 `reactive({value:'Kurja'})`, 所以修改响应式数据时，是修改 `newObj.value=xxx` 。