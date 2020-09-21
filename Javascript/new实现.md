new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象类型之一。    

## es5 实现

```javascript
function objectFactory() {
    //从Object.prototype上克隆一个对象
    var obj = new Object(),
    //取得外部传入的构造器
    Constructor = [].shift.call(arguments);

    obj.__proto__ = Constructor.prototype;
    //借用外部传入的构造器给obj设置属性
    var ret = Constructor.apply(obj, arguments);
    //确保构造器总是返回一个对象
    return typeof ret === 'object' ? ret || obj : obj;

};
```

## es6 实现

```javascript
function _new() {
    // let obj = Object.create(constructor.prototype);
    let obj = {};
    let [constructor, ...args] = [...arguments];
    obj.__proto__ = constructor.prototype;
    let result = constructor.apply(obj, args);
    if (result && typeof result === 'function' || typeof result === 'object') {
        return result;
    }
    return obj;
}
```
