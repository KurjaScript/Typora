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



### 3. 元素属性

#### 3.1 只读

- `duration`：双进度浮点数，音频的播放时长，以秒为单位。若音频不可用或者音频未加载，则返回 NaN；
- `pause`：若音频被暂停或者未开始播放，则返回 `true`；
- `ended`：音频是否播放完毕，播放完毕则返回 `true` ；
- `error`：发生错误情况下的`MediaError`对象 ；
- `currentSrc`：返回正在播放或加载的音频的`URL`地址，对应于浏览器在`source`元素中选择的文件；
- `seeking`：用户是否在音频中移动或者跳跃到新的播放点

```js
<audio preload="auto" src="music.mp3" onseeking="fn()" controls />
<script>
  var audio = document.querySelector('audio')

  function fn() {
    console.log(audio.seeking)
  }
</script>
```

#### 3.2 可读写

- `autoplay`：设置音频自动播放，或者查询音频是否设置 `autoplay`；
- `loop`：设置或者查询音频是否循环播放；
- `currentTime`：返回音频当前的播放时间点、双精度浮点数，单位为秒。音频未播放，可用于设置音频开始播放的时间点；
- `controls`：显示或隐藏音频控制面板，或者查询控制面板是否可见；
- `volume`：返回音量值，介于 `0-1` 之间的双进度浮点数，或者设置音量值；
- `muted`：设置或查询是否静音；
- `playbackRate`：设置或者查询音频的播放速度，`1` 表示正常速度，大于 `1` 表示快进，`0-1` 表示慢进，`0` 表示暂停（控制面板仍然播放，仅仅是速度为 `0`)

#### 3.3 特殊属性

##### 3.3.1 played

表示用户已经播放的音频范围，返回 `TimeRanges` 对象，其中 `TimeRanges` 对象包括一个 `length` 属性和 `start()`、`end()` 两个方法。

- `length`：获取音频范围的数量，未开始播放为 `0` ，开始播放后至少为 `1`；
- `start(index)`：获取某个音频范围的开始位置；
- `end(index)`：获取某个音频范围的结束位置。

若用户在音频中移动或者跳跃播放点，则会获得多个音频范围。

如下为一段音频，用户跳跃播放了两次，因此 `play.length` 为 `3`，其中三段音频范围分别为开始播放、第一个跳跃点、第二个跳跃点的播放范围。

![在这里插入图片描述](/Users/Kurja/Desktop/Typora/%E9%9A%8F%E6%97%B6%E4%B8%80%E9%A2%98/0efed884e80e40cdb83996d7632b709a~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

上述部分代码如下：

```html
<template>
  <div class="container">
    <audio class="music" src="audio/Mr_Rattlebone.mp3" controls>因为</audio>
    <button id="btn">console.log</button>
  </div>
</template>
<script setup lang="ts">
import { onMounted } from "vue";
onMounted(() => {
  let btn = document.querySelector("#btn");
  let audio = document.querySelector("audio");
  console.log(audio);
  console.log(btn);
  btn?.addEventListener("click", () => {
    debugger;
    const length = audio?.played.length || 1;
    console.log(`length: ${length}`);
    for (var i = 0; i < length; i++) {
      let start = audio?.played.start(i) || 0;
      let end = audio?.played.end(i) || 0;

      console.log(
        `index: ${i}, start: ${start}, end: ${end}, durations: ${end - start}s`
      );
    }
  });
});
</script>
<style scoped lang="less">
.container {
  audio {
    border: 1px solid red;
  }
  #id {
    width: 20px;
  }
}
</style>
```

##### 3.3.2 buffered

表示浏览器已经缓存的音频范围，返回 `TimeRanges` 对象，若音频已完全加载则 `buffered.length` 为 `1`， `buffered.start(0)` 为 `0`，即第一个缓存开始的区域，`buffered.end(0)` 为音频时长，即第一个缓存结束的区域。[详见 MDN 文档](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement/buffered)
