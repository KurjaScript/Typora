## React 组件基础

### 1. 组件概念

![](/Users/Kurja/Desktop/Typora/React/%E7%AC%AC%E4%B8%80%E6%AC%A1%E7%B3%BB%E7%BB%9F%E5%AD%A6%E4%B9%A0/e6c9d24egy1h4nvakt71wj21320f40t5.jpg)

### 2. 函数组件

**目标任务：**能够独立使用函数完成 react 组件的创建和渲染

**概念：**使用 JS 的函数（或箭头函数）创建的组件，就叫做函数组件。

**组件定义与渲染**

```jsx
// 定义函数组件
function HelloFn () {
  return <div>这是我的第一个函数组件</div>
}

// 定义类组件
function App () {
  return (
  	<div className="App">
    	{/* 渲染函数组件 */}
      <HelloFn />
      <HelloFn></HelloFn>
    </div>
  )
}

export default App
```

**约定说明**

1. **组件的名称必须首字母大写**，react 内部会根据这个来判断是组件还是普通的 HTML 标签；
2. **函数组件必须有返回值**，表示该组件的 UI 结构；如果不需要渲染任何内容，则返回 null；
3. 组件就像 HTML 标签一样可以被渲染到页面中。组件表示的是一段结构内容，对于函数组件来说，**渲染的内容是函数的返回值**就是对应的内容；
4. 使用函数名称作为组件标签名称，可以成对出现也可以自闭合。

### 3. 类组件

**目标任务：**能够独立完成类组件的创建和渲染。

**概念：**使用 ES6 的 class 创建的组件，叫做类（class）组件。

**组件的定义与渲染：**

```jsx
// 引入 React
import React from 'react'

// 定义类组件
class HelloC extends React.Component {
  render () {
    return <div>这是我的第一个类组件！</div>
  }
}

function App () {
  return (
  	<div className="App">
    	{/* 渲染类组件 */}
      <HelloC />
      <HelloC></HelloC>
    </div>
  )
}

export default App
```

**约定说明：**

1. **类名称也必须以大写字母开头；**
2. 类组件应该继承 React.Component 父类，从而使用父类中提供的方法或属性；
3. 类组件必须提供 render 方法，**render 方法必须有返回值，表示该组件的 UI 结构。**