### 1. computed

#### 1.1 定义

是一个计算属性，类似于过滤器，对绑定到view的数据进行处理

#### 1.2 get和set用法

```js
data: {
    firstName: '云柱',
    lastName: '火柱'
},
computed: {
    fullName: {
        //回调函数，当需要读取当前值是执行，根据相关数据计算并返回当前属性的值
        get(){
            return this.firstName + ' ' + 'this.lastName'
        },
        //监视当前属性值的变化，当属性值发生变化时执行，更新相关的属性数据
        set(val){
            //val就是fullName的最新属性值
            console.log(val)
            const names = val.split(' ');
            console.log(names)
            this.firstName = names[0];
            this.lastName = names.[1];
        }
    }
}
```

### 2. watch

#### 2.1 定义

watch是一个观察动作

#### 2.2 示例

```js
data: {
    firstName: '云柱',
    lastName: '火柱'，
    fullName: '云柱 火柱'
}
watch: {
    firstName: function(val){
        this.fullName = val + ' ' + this.lastName
    },
    this.fullName = this.firstName + ' ' + val
}
```

上面是监听firstName和lastName的变化，但仅限简单数据类型。
