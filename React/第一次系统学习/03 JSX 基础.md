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