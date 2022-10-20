1. async 函数返回一个 Promise 对象，如果在函数中 return 一个直接量，async 会把这个函数直接通过 Promise.resolve() 封装成 Promise 对象。
2. resolved，完成状态
3. pending，进行状态，表示延迟（异步）操作正在进行。