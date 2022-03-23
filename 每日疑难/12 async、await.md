### async 函数的含义和用法

**async 函数就是 Generator 函数的语法糖。**

以下是一个 Generator 函数，依次读取两个文件。

```js
let fs = require('fs');

let readFile = function (fileName) {
    return new Promise(function (resolve, reject) {
        fs.readFile(fileName, function(error, data){
            if (error) reject(error)
            resolve(data)
        })
    })
}

let gen = function* (){
    let f1 = yield readFile('/etc/fstab')
    let f2 = yield readFile('/etc/shells')
    console.log(f1.toString())
    console.log(f2.toString())
}
```

写成 async 函数是这样的，就是下面这样。

```js
let asyncReadFile = async function () {
    let f1 = await readFile('/etc/fstab')
    let f2 = await readFile('/etc/shells')
    console.log(f1.toString())
    console.log(f2.toString())
}
```

一比较就会发现，`async` 函数就是将 `Generator` 函数的星号（*）替换成 `async`，将 `yield` 替换成 `await`，仅此而已。

### async 函数的用法

async 函数返回一个 Promise 对象，可以使用 then 方法添加回调函数。**当函数执行的时候，一旦遇到 await 就会先返回，等到触发的异步操作完成，再接着执行函数体内后面的语句**。

```js
async function getStockPriceByName(name) {
    let symbol = await getStockSymbol(name)
    let stockPrice = await getStockPrice(symbol)
    return stockPrice
}

getStockPriceByName('goog').then(function (result){
    console.log(result)
})
```

上面代码是一个获取股票报价的函数，函数前面的 `async` 关键字，表明该函数内部有异步操作。调用该函数时，会立即返回一个 `Promise` 对象。

```js
function timeout (ms) {
    return new Promise((resolve) => {
        setTimeout(resolve, ms)
    })
}

async function asyncPrint(value, ms) {
    await timeout(ms)
    console.log(value)
}

asyncPrint('hello world', 50)
// 50毫秒以后，输出"hello world"
```

