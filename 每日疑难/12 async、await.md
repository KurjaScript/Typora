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

