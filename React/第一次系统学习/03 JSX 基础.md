## JSX 基础

#### 1. JSX 介绍

**目标任务：**理解什么是 JSX，JSX 的底层是什么

概念：JSX 是 JavaScript XML（HTML）的缩写，表示在 JS 代码中书写 HTML 结构。

作用：在 react 中创建 HTML 结构（页面 UI 结构）

优势：1. 采用类似于 HTML 的语法，降低学习成本，会 HTML 就会 JSX；2. 充分利用 JS 自身的可编程能力创建 HTML 结构。

注意：JSX 并不是标准的 JS 语法，是 JS 的语法扩展，浏览器默认是不识别的，脚手架内置的 [@babel/plugin-transform-react-jsx](@babel/plugin-transform-react-jsx)包，用来解析该语法。

![](/Users/Kurja/Desktop/Typora/React/%E7%AC%AC%E4%B8%80%E6%AC%A1%E7%B3%BB%E7%BB%9F%E5%AD%A6%E4%B9%A0/e6c9d24egy1h4nn4j48k7j20ku0d4t9q.jpg)

### 2. JSX 中使用 js 表达式

**目标任务：**能在 JSX 中使用表达式

**语法** ：`{ JS 表达式 }`

```jsx
const name = '云柱'
<h1>你好，我叫{name}</h1> // <h1>你好，我叫云柱</h1>
```

**可以使用的表达式**：

- 字符串、数值、布尔值、null、undefined、object([] / {})
- 1 + 2、'abc'.split('')、['a', 'b'].join('-')
- fn()

**特别注意**

> if 语句 /switch-case 语句/ 变量声明语句，这些叫做语句，不是表达式，不能出现在 `{}`中！

### 3. JSX 列表渲染

**目标任务：**能够在 JSX 中实现列表渲染。

页面的构建离不开重复的列表结构，比如歌曲列表，商品列表等，vue 中用的是 v-for，react 通过使用数组的 `map` 方法实现。

```jsx
// 来个列表
const songs = [
  { id: 1, name: '云柱'},
  { id: 2, name: '火柱'},
  { id: 3, name: 'Kurja'}
]

function App() {
  return (
  	<div className="App">
    	<ul>
      	{ songs.map(item => <li>{item.name}</li>) }
      </ul>
    </div>
  )
}

export default App
```

注意点：需要为遍历项添加 `key` 属性

![](/Users/Kurja/Desktop/Typora/React/%E7%AC%AC%E4%B8%80%E6%AC%A1%E7%B3%BB%E7%BB%9F%E5%AD%A6%E4%B9%A0/e6c9d24egy1h4nqy68sg6j216s04lt9h.jpg)

1. key 在 HTML 结构中是看不到的，是 react 内部用来进行性能优化时使用；
2. key 在当前列表中要唯一的字符串或者数值(String / Number);
3. 如果列表中有像 id 这种的唯一值，就用 id 来作为 key 值；
4. 如果列表中没有像 id 这种唯一的值，就可以使用 index （下标）来作为 key 值。

### 4. JSX 条件渲染

**目标任务：**能够在 JSX 中实现条件渲染。

作用：根据是否满足条件生成 HTML 结构，比如 Loading 效果。

实现：可以使用**三元运算符**或**逻辑与(&&)运算符**

```jsx
// 来个布尔值
const flag = true
function App() {
  return (
  	<div className="App">
    { /* 条件渲染字符串 */ }
    { flag ? 'react' : 'vue'}
    { /* 条件渲染标签组件 */ }
		{ flag ? <span>this is span</span> : null}      
    </div>
  )
}
export default App
```

### 5. JSX 样式处理

**目标任务：**能在 JSX 中实现 css 样式处理

- 行内样式 -style

  ```jsx
  function App() {
    return (
    	<div className="App">
      	<div style={{ color: 'red' }}>this is a div</div>
      </div>
    )
  }
  
  export default App
  ```

- 行内样式-style-更优写法

  ```jsx
  const styleObj = {
  	color: red
  }
  function App() {
    return (
    	<div className="App">
      	<div style={ styleObj }>this is a div</div>
      </div>
    )
  }
  
  export default App
  ```

- 类名 -className （推荐）

  ```jsx
  .title {
    font-size: 30px;
    color: blue;
  }
  ```

- 类名 -className -动态类名控制

  ```jsx
  import './app.css'
  const showTitle = true
  function App() {
    return (
    	<div className="App">
      	<div className={ showTitle ? 'title' : ''}>this is a div</div>
      </div>
    )
  }
  ```

### 6. JSX 注意事项

**目标任务：**掌握 JSX 在实际应用时的注意事项

1. JSX 必须有一个根节点，如果没有根节点，可以使用 `<></>` (幽灵节点) 替代；
2. 所有标签必须形成闭合，成对闭合或者自闭合都可以；
3. JSX 中的语法更加贴近 JS 语法，属性名采用驼峰命名法 `class -> className`、`for -> htmlFor`；
4. JSX 支持多行（换行），如果需要换行，需使用 `()`包裹，防止 bug 出现。

