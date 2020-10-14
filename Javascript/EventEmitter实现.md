## EventEmitter实现   

```javascript
// 我们以ES6类的形式写出来
class EventEmitter {
    constructor() {
        // 事件对象，存储订阅的type类型
        this.events = Object.create(null);
    }
    on(type, cb) {
        let events = this.events;
        // 如果该type类型存在，就继续向数组中添加回调cb
        if (events[type]) {
            events[type].push(cb);
        } else {
            // type类型第一次存入的话，就创建一个数组空间并存入回调cb
            event[type] = [cb];
        }
    },
    emit(type, ...args) {
        // 遍历对应type订阅的数组，全部执行
        if (this.events[type]) {
            this.events[type].forEach(listener => {
                listener.call(this, ...args);
            });   
        }
    }
    off(type, cb) {
        let events = this.events;
        if (events[type]) {
            events[type] = events[type].filter(listener => {
                // 过滤用不到的回调cb
                return listener !== cb && listener.listen !== cb;
            });
        }
    }
    once(type, cb) {
        function wrap() {
            cb(...arguments);
            this.off(type, wrap);
        }
        // 先绑定，调用后删除
        wrap.listen = cb;
        // 直接调用on方法
        this.on(type, wrap);
    }
}
```
