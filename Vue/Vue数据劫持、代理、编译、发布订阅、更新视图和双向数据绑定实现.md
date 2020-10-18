## Vue原理简单实现

```javascript
class Vue {
    constructor(options) {
        this.$options = options;
        this.$el = options.el;
        this.$data = options.data;
 
        new Observer(this.$data); // 数据劫持
        Object.keys(this.$data).forEach(key => {
            this.proxyData(key);  // 数据代理
        });
        new Compiler(this.$el, this); // 编译模板
    }

    proxyData(key) {
        Object.defineProperty(this, key, {
            enumerable: true, // 是否可以枚举
            configurable: true, // 是否可以配置
            get() {
                return this.$data[key];
            },
            set(newVal) {
                this.$data[key] = newVal;
            }
        })
    }
}

class Observer {
    constructor(data) {
        this.data = data;
        Object.keys(data).forEach(key => {
            defineReactive(this.data, key, this.data[key]);
        })
    }
    // 监听数据变化，收集订阅者，并添加到订阅器
    defineReactive(obj, key, value) {
        const dep = new Dep(); // 新建一个订阅者
        Object.defineProperty(obj, key, {
            enumerable: true,
            configurable: true,
            get() {
                if (Dep.target) dep.addSub(Dep.target); // 添加订阅者到订阅器
                return value;
            },
            set(newVal) {
                if (value === newVal) return;
                dep.notify(); // 通知订阅者更新
                value = newVal;
            }
        })
    }
}
// 消息订阅器
class Dep {
    constructor() {
        this.subs = [];
    }

    addSub(sub) {
        this.subs.push(sub);
    }

    notify() {
        this.subs.forEach(sub => sub.update(););
    }
}
// 订阅者
class Watch {
    constructor(node, name, vm) {
        this.node = node;
        this.name = name;
        this.vm = vm;
        Dep.target = this;
        this.update(); // 触发getter，添加订阅者
        Dep.target = null;
    }

    update() {
        this.node.textContent = this.vm[this.name];
    }
}

const reg = /\{\{.+\}\}/;

class Compiler {
    constructor(el, vm) {
        this.el = document.querySelector(el);
        this.vm = vm;
        this.frag = this.createFrag(); // 将已有的el元素的所有子元素转成文档碎片
        this.el.appendChild(this.frag); // 将文档碎片重新添加到dom树
    }

    createFrag() {
        let frag = document.createDocumentFragment();
        let child;
        while(child = this.el.firstChild) {
            this._compiler(child);
            frag.appendChild(child); // 给文档碎片添加节点时，该节点会自动从dom中删除
        }
        return frag;
    }

    _compiler(node) {
        if (node.nodeType === 1) { // 标签节点
            const attrs = node.attributes; // 获取dom上的所有属性,是个类数组
            if (attrs.hasOwnProperty('v-model')) {
                const name = attrs['v-model'].nodeValue;
                node.addEventListener('input', e=> {
                    this.vm[name] = e.target.value;
                })
                new Watch(node, name, this.vm);
            }
        }
        if (node.nodeType === 3) { // 文本节点
            if (reg.test(node.nodeValue)) {
                const name = RegExp.$1.trim(); // 去除插值运算符中的变量
                new Watch(node, name, this.vm);
            }
        }
    }
}
```
