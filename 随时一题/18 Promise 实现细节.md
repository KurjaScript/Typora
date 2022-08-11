## Promise 实现细节

### 1. 手写前需要了解这些

#### 1.1 什么是宏任务与微任务？

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

**如何理解 script（整体代码块）是个宏任务**

实际上，如果同时存在两个 script 代码块，会首先执行第一个 script 代码块中的同步代码，如果这个过程中创建了微任务并进入微任务队列，第一个 script 同步代码执行完之后，会首先去清空微任务队列，再去开启第二个 script 代码块的执行。所以这里应该就理解 script （整体代码块）为什么会是宏任务。

#### 1.2 什么是 EventLoop？

*![](/Users/Kurja/Desktop/Typora/%E9%9A%8F%E6%97%B6%E4%B8%80%E9%A2%98/e6c9d24egy1h52ww9o9d0j20dn0ep3yy.jpg)**

因为首次执行宏任务队列会有 script（整体代码块）任务，所以实际上就是 JS 解析完成后，在异步任务中，会先执行完所有的微任务，这也是很多面试题喜欢考察的。需要注意的是，**新创建的微任务会立即进入微任务队列排队执行，不需要等待下一次轮巡。**

#### 1.3 什么是 Promise A+ 规范

目前我们使用的 Promise 是基于 [Promise A+ 规范](https://promisesaplus.com/) 实现的。

检验一份手写 Promise 靠不靠谱，通过 Promise A+ 规范自然是基本要求，这里我们可以借助 [promises-aplus-tests](https://link.juejin.cn/?target=https%3A%2F%2Fwww.npmjs.com%2Fpackage%2Fpromises-aplus-tests) 来检测我们的代码是否符合规范，后面我会讲到如何使用它。