### 1. 前言

、在跟着教程搭建好项目之后，你会收获：

- 快速搭建一个文档/博客，后期只需要修改 markdown 内容和导航参数，即可一键打包生成页面。
- 页面具有非常好的加载性能和搜索引擎优化（SEO），其他页面只会在用户浏览到的时候才按需加载。
- 自动生成全局搜索、记录上次修改时间等功能。
- 可嵌入 vue 组件或跳转至其他页面，可定制自己的样式模版便于扩展。
- 一键免费部署到 github.pages，**无需服务器即可拥有自己的在线文档/博客**。

vuepress 是 vue 驱动的静态网站生成器。项目整体以 Markdown 为中心结构，可以让你用最少的配置完成文档/博客写作。但是，vuepress 官方文档对初学者不太友好，里面的主题是什么东西？配置里面为什么没有页面和导航设置？首页和其他页面到底在哪？为什么主题还可以导出？浏览 vuepress 官方文档的最大感受就是**看不懂**。

### 2. 如何制作文档结构？

首先要明确很重要的两个概念：

1. **vuepress 项目的文档结构，都必须按照官方文档的格式进行制作。**比如你想修改整体的主题颜色，你就必须修改 styles 文件夹下的 palette.style。你想在 markdown 中添加 vue 组件，vue 组件必须放在 components 文件夹下，诸如此类。**等到 vuepress 在打包的时候，它会遍历特定的路径和文件名称，读取内容生成页面结构和样式。**
2. 我们这里使用的，都是官方的默认主题。**主题的意思就是页面的样式和结构。**你选择了某一个主题，你的文档内容结构就得根据当前主题进行设置。

接下来开始我们的制作。首先打开 git 客户端，创建一个文件夹，并且进入文件夹。

```bash
mkdir testproject // 创建文件夹，文件夹名字最好为小写
cd testproject  // 进入文件夹
```

然后将 VuePress 安装为本地依赖

我们已经不再推荐全局安装 VuePress

```bash
yarn add -D vuepress # npm install -D vuepress
```

接下来初始化项目，创建 package.json 文件

```bash
npm init -y
```

创建完成后，进入 package.json 文件，在 script 中添加两条命令

```
"script": {
	"dev": "vuepress dev docs", // 用于实时预览
	"build": "vuepress build docs" // 用于打包项目
}
```

接着我们需要创建 vuepress 项目的文件夹和文件，先创建好整体的架构再跟着教程了解里面的内容。

```
// 下面没有文件类型后缀的都是文件夹
// 部分内容并不是必须的，想自己定制的话可以参考官方文档。这里是按照我的思路写的。

├── docs
│   ├── .vuepress  //存放核心内容的文件夹
│   │   ├── components  //存放你需要添加的vue组件
│   │   ├── public  //存放静态文件，如图片等
│   │   ├── styles  //存放需要定制的样式
│   │   │   └── palette.styl  //配置页面主题颜色的文件
│   │   └── config.js   //设定顶部导航栏、侧边导航栏等项目配置的核心文件
│   ├── pages   //存放markdown文件，用于设置其他页面内容
│   ├── README.md   //首页展示用的markdown文件
├── deploy.sh     //之后用于编写上传、发布脚本的文件
└── package.json  //之前创建的Node.js项目描述文件
```

对整体项目结构有一定了解后，我们再了解如何调整每个模块。之后就可以对整个项目得心应手了。[官方的目录结构](https://vuepress.vuejs.org/zh/guide/directory-structure.html) 较为丰富，可自行参照。

### 3. 页面的具体内容如何设置

#### 3.1 修改页面整体设置

首先我们先设定网页与浏览器标签栏相关的一些设置，直接在我们的 `config.js` 文件添加下面的代码。

```ts
module.exports = {
    title: '云柱首页', // 显示在左上角的网页名称以及首页在浏览器标签显示的 title 名称
    description: '云柱的前端记录', // meta 中的描述文字，用于 SEO
    // 注入到当前页面的 HTML <head> 中的标签
    head: [
        ['link', 
            { rel: 'icon', href: '/cloud_pillar.png'}
            // 浏览器的标签栏的网页图标，第一个 '/' 会遍历 public 文件夹的文件
        ],
    ],
}
```

修改完后，在 `doc/.vuepress/public` 文件夹里面放置我们的 logo 图片。

![](/Users/Kurja/Desktop/Typora/%E5%B7%A5%E5%85%B7%E5%BA%93/VuePress/e6c9d24egy1h4lb5a3hctj20fc07iwek-20220727110959727.jpg)

然后运行 `yarn dev`， 打开 `http://localhost:8080` 会得到如下效果。左上角的图标和页面名称就设置好了：

![](/Users/Kurja/Desktop/Typora/%E5%B7%A5%E5%85%B7%E5%BA%93/VuePress/e6c9d24egy1h4lbaisus9j20lv062wep.jpg)

