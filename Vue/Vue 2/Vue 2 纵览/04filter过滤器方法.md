### 1. 基础

#### 1.1 作用

过滤器方法的作用是把需要在视图中渲染的数据进行二次或多次的处理

#### 1.2 语法

过滤器应该被添加在JavaScript表达式的尾部，由"管道"符号指示。

### 2. 在哪里定义过滤器

#### 2.1 在一个组件的选项中定义本地过滤器

```js
filters: {
    capitalize: function(value){
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice
    }
}
```

#### 2.2 在创建Vue实例之前全局定义过滤器

```js
Vue.filter('capitalize', function (value) {
    if (!value) return ''
    value = value.toString()
    return value.charAt(0).toUpperCase() + value.slice(1)
})

new Vue({
    // ...
})
```

#### 2.3 注意

- 当全局过滤器重名时，会采用局部过滤器；
- 过滤器中的方法没有挂载到实例上，`this`是`window`；
- 过滤器方法的名称可以称为和methods中的方法名称一样，不会冲突。

### 3. 过滤器用在两个地方

#### 3.1 双花括号插值

`{{message | capitalize}}`

#### 3.2 v-bind表达式

`<div v-bind="rawId"></div>`

过滤函数总接收表达式的值作为第一个参数。拿上面双花括号插值的表达式举例：capitalize过滤器函数将会收到message的值作为第一个参数。

### 4. 过滤器可以串联

#### 4.1 单参数

 `{{message | filterA | filterB}}`

`filterA`被定义为接收单个参数的过滤器函数，表达式message的值将作为参数传入到函数中；然后继续调用同样被定义为接收单个参数的过滤器函数`filterB`,将`filterA`的结果传递到`filterB`中。

#### 4.2 多参数

`{{message | filterA('arg1', arg2)}}`

`filterA`被定义为接收三个参数的过滤器函数，其中`message`的值作为第一个参数，普通字符串`'arg1'`作为第二个参数，表达式`arg2`作为第三个参数。