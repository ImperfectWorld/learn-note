## promise
```javascript
// new Promise((resolve, reject) => {resolve(data)}).then()
// promise 三种状态 pending, resolve(fulfilled), reject
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

class Promise {
    static resolve(value) {
        if (value && value.then) {
            return value;
        }
        return new Promise(resolve => resolve(value));
    },
    constructor(fn) {
        this.state = PENDING;
        this.value = undefined;
        this.reason = undefined;

        // 存放回调函数
        this.onResolvedCallbacks = [];
        this.onRejectedCallbacks = [];

        let resolve = (value) => {
            if (this.state === PENDING) {
                this.state = FULFILLED;
                this.value = value;
                this.onResolvedCallbacks.forEach(fn => fn());
            }
        }

        let reject = (reason) => {
            if (this.state === PENDING) {
                this.state = REJECTED;
                this.reason = reason;
                this.onRejectedCallbacks.forEach(fn => fn());
            }
        }

        try {
            fn(resolve, reject);
        } catch (error) {
            reject(error);
        }
    }

    then(onFulfilled, onRejected) {
        if (this.state === FULFILLED) {
            onFulfilled(this.value);
        }

        if (this.state === REJECTED) {
            onRejected(this.reason);
        }

        if (this.state === PENDING) {
            this.onResolvedCallbacks.push(() => {
                onFulfilled(this.value);
            })
            this.onRejectedCallbacks.push(() => {
                onRejected(this.reason);
            })
        }
    }
}
```

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
