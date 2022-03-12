# 微前端js沙箱原理

> 快照沙箱： 两个子应用
```javascript
class SnapshotSandbox {
    constructor() {
        this.proxy = window;
        this.modifyPropsMap = {}; // 记录window上的修改
        this.active();
    }
    active() { // 激活
        this.windowSnapshot = {}; // 拍照
        for (const prop in window) {
            if (window.hasOwnProperty(prop)) {
                this.windowSnapshot[prop] = window[prop];                
            }
        }
        Object.keys(this.modifyPropsMap).forEach(prop => {
            window[prop] = this.modifyPropsMap[prop];
        })
    }
    inactive() { // 失活
        for (const prop in window) {
            if (window.hasOwnProperty(prop)) {
                if (window[prop] !== this.windowSnapshot[prop]) {
                    this.modifyPropsMap[prop] = window[prop];
                    window[prop] = this.windowSnapshot[prop];
                }               
            }
        }
    }
}

let sandbox = new SnapshotSandbox();
((window) => {
    window.a = 1;
    window.b = 2;
    console.log(window.a, window.b);
    sandbox.inactive();
    console.log(window.a, window.b);
    sandbox.active();
    console.log(window.a, window.b);
})(sandbox.proxy)
```

> 代理沙箱【proxy】，多个子应用：不同应用用不同代理处理
