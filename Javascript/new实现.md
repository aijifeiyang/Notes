new 运算符创建一个用户定义的对象类型的实例或具有构造函数的内置对象类型之一。    

## es5 实现

function objectFactory() {

    var obj = new Object(),

    Constructor = [].shift.call(arguments);

    obj.__proto__ = Constructor.prototype;

    var ret = Constructor.apply(obj, arguments);

    return typeof ret === 'object' ? ret : obj;

};

## es6 实现

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
