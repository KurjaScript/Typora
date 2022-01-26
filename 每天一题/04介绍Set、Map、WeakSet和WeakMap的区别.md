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


#### WeakSet

WeakSet对象允许你将**弱引用对象**储存在一个集合中

**弱引用与强引用相对，是指不能确保其引用的对象不会被垃圾回收器回收的引用。**

##### WeakSet与Set的区别：

- WeakSet只能储存对象引用，不能存放值，而Set对象都可以
- WeakSet对象中储存的对象值都是弱引用，**即垃圾回收机制不考虑WeakSet对象的应用，如果没有其他变量或属性引用这个对象值，则这个对象将会被垃圾回收掉**(不考虑该对象还存于WeakSet中)，所以，WeakSet对象里有多少个成员元素，取决于垃圾回收机制有没有运行，运行前后成员个数可能不一致，遍历结束之后，有的成员可能取不到了(被垃圾回收了)，WeakSet对象是无法被遍历的(ES6规定WeakSet不可遍历)，也没有办法拿到它包含的所有元素。

属性：

- constructor: 构造函数，任何一个具有Iterable接口的对象，都可以作参数

  ```js
  const arr = [[1, 2], [3, 4]]
  const weakset = new WeakSet(arr)
  console.log(weakset)
  ```

  ![image-20220126141043762](C:\Users\GunKing\AppData\Roaming\Typora\typora-user-images\image-20220126141043762.png)

然而我在浏览器控制台的调试结果是这样的，没有顺序打印，何故？然而，用其他数据测试则是正常

![image-20220126141007136](C:\Users\GunKing\AppData\Roaming\Typora\typora-user-images\image-20220126141007136.png)

![image-20220126141114833](C:\Users\GunKing\AppData\Roaming\Typora\typora-user-images\image-20220126141114833.png)

方法：

- add(value): 在WeakSet对象中添加一个元素value；

- has(value): 判断WeakSet对象中是否包含value；

- delete(value): 删除元素value；

- clear(): 清空所有元素，**注意该方法已废除**

  ```js
  let ws = new WeakSet()
  let obj = {}
  let foo = {}
  
  ws.add(window)
  ws.add(obj)
  
  ws.has(window) //true
  ws.has(foo) //false
  
  ws.delete(window) //true
  ws.has(window) //false
  ```

#### 3. 字典(Map)