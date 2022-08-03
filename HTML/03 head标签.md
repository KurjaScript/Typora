## `<head>` 标签里有什么？Metadata-HTML中的元数据

head 标签里的内容不会在页面中显示出来。head 标签包含了页面的`<title>`、指向 CSS 的链接、指向自定义的图标的链接和其它**元数据**（描述 HTML 的数据，比如，作者和描述文档的关键字）等信息。

### 1. 什么是 HTML `<head>` 标签

```html
<head>
	<meta charset="utf-8">
  <title>我的测试页面</title>
</head>
```

大型页面的 head 会包含很多元数据。可以用 [开发者工具](https://developer.mozilla.org/zh-CN/docs/Learn/Common_questions/What_are_browser_developer_tools#%E5%A6%82%E4%BD%95%E5%9C%A8%E6%B5%8F%E8%A7%88%E5%99%A8%E4%B8%AD%E6%89%93%E5%BC%80%E5%BC%80%E5%8F%91%E8%80%85%E5%B7%A5%E5%85%B7)查看网页的 head 信息。这里只是初步介绍几项 head 中重要的常用元素。

### 2. 添加标题

`<title>` 元素可以为文档添加标题，别和 `<h1>` 元素是为 body 添加标题的。

- `<h1>` 元素在页面加载完毕时显示在页面中，通常只出现一次，用来标记页面内容的标题（故事名称、新闻摘要，等等）。
- `<title>` 元素是一项元数据，用于表示整个 HTML文档的标题（而不是文档内容）。