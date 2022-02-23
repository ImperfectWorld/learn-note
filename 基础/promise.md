### promiseAll 实现

```
// 实现promise.all 用法: promise.all([promiseList]).then(res => {});
// 1. 返回一个promise return new Promise(res, rej)
// 2. 存储返回值 let result = []
// 3. 遍历promises

const promiseAll = (promises) => {
    return new Promise((resolve, reject) => {
        if (!Array.isArray(promises)) {
            throw new TypeError('promises must be an array');
        }
        let result = [];
        let count = 0;
        promises.forEach((promise, index) => {
            promise.then(res => {
                result[index] = res;
                count++;
                count === promises.length && resolve(result);
            }, err => {
                reject(err);
            })
        })
    })
};
```
