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

#### 2.2 监听简单数据类型

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
    lastName: function(val){
    	this.fullName = this.firstName + ' ' + val   
    }
}
```

上面是监听firstName和lastName的变化，但仅限简单数据类型。

#### 2.3 监听复杂数据类型

监听复杂数据类型需用**深度监听**

```js
data(){
    return {
        'first': {
            second: 0
        }
    }
}
watch: {
    secondChange: {
        handler(oldVal, newVal){
            console.log(oldVal)
            console.log(newVal)
        },
        deep: true
    }
}
```

`console.log`打印结果显示，`oldVal`和`newVal`的值是一样的，所以深度监听虽然可以监听到对象的变化，但无法监听到具体对象里面那个属性的变化。

`oldVal`和`newVal`值一样的原因是它们索引同一个对象/数组。`Vue`不会保留修改之前值的副本。

**vm.$watch的深度监听**

![image-20220218023135191](C:\Users\Kurja\AppData\Roaming\Typora\typora-user-images\image-20220218023135191.png)

**深度监听对应的函数名必须是handler**，否则无效果，因为`watcher`里面对应的是对`handler`的调用。
