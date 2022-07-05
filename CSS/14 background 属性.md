### background-color 与 background 设置背景颜色的区别

background-color 设置的只是背景色，background 设置的是整个背景。

当元素本身设置了 background-image 属性时，如果设置了 background-color，图片不会被覆盖，**background-color 会在 image 底层**；**如果设置的是 background，那么图片会被颜色覆盖掉**。

### background-attachment

定义 background-image 是否跟随容器的滚动而滚动。对 background-color 无效。

`background-attachment = scroll | fixed | local`

当设置 `background-attachment: fixed` 时，设置的 background-image 背景不随元素滚动而滚动。

### background-position

定义 background-image 在容器的位置。对 background-color 无效。

`Background-position = [left | center | right | top | bottom | length-percentage]`

```css
body { background-position: right top } /* 100% 0% */
body { background-position: top center } /* 50% 0% */
body { background-position: center } /* 50% 50% */
body { background-position: bottom } /* 50% 100% */
```

### Visibility: hidden 与 display: none 对 background-image 的影响

background-image 对应的是一张静态资源图片，当页面渲染前会请求这张图片资源回来进行渲染。

visibility: 元素仍然存在DOM树中，只是不显示而已；display: none，元素不存在与DOM树中。

当元素visibility: hidden时，这张图片资源仍然会请求，但是不会显示出来；当元素设置display: none时，这张图片资源不会请求，也不会显示。

还有一种场景：**如果页面出现了滚动条，那么滚动条下面（即视图之外）的background-image 静态资源会不会请求呢？会的。**只要元素存在于 DOM 树上，不论显示还是隐藏，其依赖的 background-image 资源都将会请求回来。

**从这里可以引发一些对资源请求的思考了**

