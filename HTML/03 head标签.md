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

### 3. 元数据：`<meta>` 元素

**元数据就是描述数据的数据**，HTML 有一个“官方的”方式来为一个文档添加数据——`<meta>` 元素。

有很多不同种类的 `<meta>` 元素可以被包含进你的页面的 `<head>` 元素，但是这里先解释一些常见到的类型。

#### 3.1 指定你的文档中字符的编码

```html
<meta charset="utf-8">
```

这个元素指定了文档的字符编码——在这个文档中被允许使用的字符集。`utf-8` 是一个通用的字符集，它包含了任何人类语言中的大部分的字符。这意味着该 web 页面可以显示任何的语言。**所以应该在每一个页面都使用这个设置。**

#### 3.2 添加作者和描述

许多 `<meta>` 元素包含了 `name` 和 `content` 属性：

- `name` 指定了 meta 元素的类型；说明该元素包含了什么类型的信息。
- `content` 指定了实际的元数据的内容。

这两个 meta 元素对于定义页面的作者和提供页面的简单描述是很有用的。如下示例：

```html
<meta name="author" content="Chris Mills">
<meta name="description" content="The MDN Web Docs Learning Area aims to provide
complete beginners to the Web with all they need to know to get
started with developing web sites and applications.">
```

指定作者在某些情况下是很有用的：如果你需要联系页面的作者，问一些关于页面内容的问题。一些内容管理系统能够自动获取页面作者的信息，然后用于某些用途。

指定包含关于页面内容的关键字的页面内容的描述是很有用的，因为它可能或让你的页面在搜索引擎的相关的搜索出现得更多（这些行为在术语上被称为：**搜索引擎优化，或 SEO。**）