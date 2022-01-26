Set和Map主要的应用场景在于**数据重组**和**数据储存**

Set是一种叫做**集合的**数据结构，Map是一种叫做**字典**的数据结构

#### 1. 集合（Set）

Set本身是一种构造函数，用来生成Set数据结构，类似于数组，但成员是唯一且无序的，没有重复的值。

```js
new Set([iterable])
```

举个例子：

```js
const s = new Set()
[1, 2, 3, 4, 3, 2, 1].forEach(x => s.add(x))
for (let i of s){
    console.log(i)  //1 2 3 4 
}
// 数组去重
let arr = [1, 2, 3, 2, 1, 1]
[...new Set(arr)] //[1, 2, 3]
```

- Set实例属性

  - constructor: 构造函数

  - size: 元素数量

    ```js
    let set = new Set([1, 2, 3, 2, 1])
    console.log(set.length) // undefined
    console.log(set.size)   // 3
    ```

- Set实例方法

  - 操作方法

    - add(value): 新增，相当于array里的push；

    - delete(value): 若存在就删除集合中的value；

    - has(value): 判断集合中是否存在value

    - clear(): 清空集合

      ```js
      let set = new Set()
      set.add(1).add(2).add(1)
      
      set.has(1) //true
      set.has(3) //false
      set.delete(1)
      set.has(1) //false
      ```

      `Array.from`方法可以**将Set结构转化为数组**

      ```js
      const items = new Set([1, 2, 3, 2])
      const array = Array.from(items)
      console.log(array) //[1, 2, 3]
      ```

      `...`运算扩展符**将Set结构转化为数组更直观简洁**

      ```js
      const items = new Set([1, 2, 3, 2])
      const array = [...items]
      console.log(array) //[1, 2, 3]
      ```

      

  - 遍历方法(遍历顺序为插入顺序)

    - keys(): 返回一个包含集合中所有键的迭代器；

    - values(): 返回一个包含集合所有值的迭代器；

    - entries(): 返回一个包含Set对象中所有元素的键值对迭代器；

    - forEach(callbackFn, thisArg): 用于对集合成员执行callbackFn操作，如果提供了thisArg参数，回调中的this会是这个参数，没有返回值。

      ```js
      let set = new Set([1, 2, 3])
      console.log(set.keys()) //SetIterator {1, 2, 3}
      console.log(set.values()) //SetIterator {1, 2, 3}
      console.log(set.entries()) //SetIterator {1 => 1, 2 => 2, 3 => 3}
      
      for (let item of set.keys()){
          console.log(item); //1 2 3
      }
      
      for (let item of set.entries()){
          console.log(item); //[1, 1] [2, 2] [3, 3]
      }
      
      set.forEach((value, key) => {
          console.log(key + ':' + value); //1:1  2:2  3:3
      })
      
      console.log([...set]) // [1, 2, 3]
      ```

      Set可默认遍历，默认迭代器生成函数values()方法

      ```js
      Set.prototype[Symbol.iterator] === Set.prototype.values //true
      ```

      所以，Set可以使用map、filter方法

      ```js
      let set = new Set([1, 2, 3])
      set = new Set([...set].map(item => item * 2))
      console.log([...set]) //[2, 4, 6]
      
      set = new Set([...set].filter(item => (item >= 4))
      console.log([...set]) // [4, 6]
      ```

      因此，Set很容易实现交集(Intersect)、并集(Union)、差集(Difference)

      ```js
      let set1 = new Set([1, 2, 3])
      let set2 = new Set([4, 3, 2])
      
      let intersect = new Set([...set1].filter(value => set2.has(value)))
      let union = new Set([...set1, ...set2])
      let difference = new Set([...set1].filter(value => !set2.has(value)))
      ```

      