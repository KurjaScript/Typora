### BFC 是什么

`BFC` 全称：`Block Formatting Context`， 名为 "块级格式化上下文"。`BFC`是一个完全独立的空间（布局环境），让空间里的子元素不会影响到外面的布局。

#### 如何触发 BFC

- `overflow: hidden`
- `display: inline-block`
- `position: absolute`
- `position: fix`
- `display: table-cell`
- `display: flex`