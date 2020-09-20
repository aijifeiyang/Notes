bind() 方法会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，之后的一序列参数将会在传递的实参前传入作为它的参数。(来自于 MDN )    

## es5 实现

```javascript
Function.prototype.bind2 = function (context) {

    if (typeof this !== "function") {
      throw new Error("Function.prototype.bind - what is trying to be bound is not callable");
    }

    var self = this;
    var args = Array.prototype.slice.call(arguments, 1);

    var fNOP = function () {};

    var fBound = function () {
        var bindArgs = Array.prototype.slice.call(arguments);
        return self.apply(this instanceof fNOP ? this : context, args.concat(bindArgs));
    }

    fNOP.prototype = this.prototype;
    fBound.prototype = new fNOP();
    return fBound;
}
```

## es6 实现

```javascript
Function.prototype.bind = function(context, ...args) {
    // context为要改变的执行上下文
    // ...args为传入bind函数的其余参数
    return (...newArgs) => {
        // 这里返回一个新的函数
        // 通过调用call方法改变this指向并且把老参和新参一并传入
        return this.call(context, ...args, ...newArgs);
    }
};
```
