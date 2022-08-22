## HTML5 之 Audio 标签

### 1. 标签属性

- `autoplay`：音频会尽快自动播放，不会等待整个音频下载完成；
- `controls`：浏览器提供包括声音、播放进度、播放暂停的控制面板（不同浏览器不一致），用户可以控制音频播放；
- `loop`：循环播放音频；
- `muted`：是否静音，默认值为 false，表示有声音；
- `preload`：
- 预加载，包括 `auto` 、 `metadata` 和 `none` 三个参数值，`auto` 表示加载音频，`metadata` 表示不加载音频，但是需要获取音频元数据（如音频长度），`none` 表示不加载音频。若指定为空字符串，则等效于 `auto`。注意 `autoplay` 属性优先级高于 `preload`，若 `autoplay` 被指定，则会忽略此属性，浏览器将加载音频以供播放；
- `src`：嵌入音频 url



### 2. 设置多个音频资源

不同浏览器支持的音频播放格式有所不同，一般为了兼容效果，会放置多种音频格式，浏览器会自上而下选择符合的音频格式。

```js
<audio controls>
  <source src="music.ogg" type="audio/ogg" />
  <source src="music.mp3" type="audio/mpeg" />
  <source src="music.wav" type="audio/Wav" />
  当前浏览器不支持audio标签
</audio>
```

