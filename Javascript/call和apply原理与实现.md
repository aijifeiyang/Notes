call() 方法在使用一个指定的 this 值和若干个指定的参数值的前提下调用某个函数或方法。    
apply() 方法与call() 相似，只是第二个参数是一个数组。

## es5 实现写法
```javascript
Function.prototype.call = function (context) {
    var context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

    var result = eval('context.fn(' + args +')');

    delete context.fn
    return result;
}

Function.prototype.apply = function (context, arr) {
    var context = Object(context) || window;
    context.fn = this;

    var result;
    if (!arr) {
        result = context.fn();
    }
    else {
        var args = [];
        for (var i = 0, len = arr.length; i < len; i++) {
            args.push('arr[' + i + ']');
        }
        result = eval('context.fn(' + args + ')')
    }

    delete context.fn
    return result;
}
```

## es6 实现写法
```javascript
// 开头也可以这样写
//Function.prototype.call = function () {
//  let [thisArg, ...args] = [...arguments]
//  ... }
Function.prototype.call = function(context, ...args) {
    // 执行上下文都保证是对象类型，如果不是就是window
    context = Object(context) || window;
    // 创建一个额外的变量当做context的属性
    const fn = Symbol();
    // 给这个fn属性赋值为当前的函数
    context[fn] = this;
    // 执行函数把...args传入
    const result = context[fn](...args);
    // 删除使用过的fn属性
    delete context[fn];
    // 返回函数执行结果
    return result;
};

Function.prototype.apply = function(context, arrArgs) {
    context = Object(context) || window;
    const fn = Symbol();
    context[fn] = this;
    // 需要把传入apply的数组进行展开运算
    // 所以在这里性能会有些消耗相比call来讲
    const result = context[fn](...arrArgs);
    delete context[fn];
    return result;
}
```
