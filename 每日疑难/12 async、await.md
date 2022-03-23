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

