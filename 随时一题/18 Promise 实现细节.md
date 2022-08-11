## Promise 实现细节

### 手写前需要了解这些

#### 什么是宏任务与微任务

JS 是单线程，但是一些高耗时操作会带来进程阻塞问题。为了解决这个问题，JS 有两种任务的执行模式：**同步模式（Synchronous）和异步模式（Asynchronous）**。

在异步模式下，创建**异步任务主要分为宏任务和微任务两种**。ES6 规范中，宏任务（Macrotask）称为 Task，微任务（Microtask）称为 Jobs。**宏任务是由宿主发起的，而微任务是由 JS 自身发起的。**

**宏任务与微任务的几种创建方式**

| 宏任务（Macrotask）      | 微任务（Microtask）             |
| ------------------------ | ------------------------------- |
| setTimeout               | requestAnimationFrame（有争议） |
| setInterval              | MutationObserver（浏览器环境）  |
| MessageChannel           | Promise.[ then/catch/finally ]  |
| I/O，事件队列            | process.nextTick（Node环境）    |
| setImmediate（Node环境） | queueMicrotask                  |
| script（整体代码块）     |                                 |





