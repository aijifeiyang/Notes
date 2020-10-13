## instanceof实现

```javascript
function instanceOf1013 (a, b) {
    let sub = a._proto_;
    let sup = b.prototype;
    while (true) {
        if (sub === null) return false;
        if (sub === sup) return true;
        sub = sub._proto_;
    }
}
```

## Object.create实现

```javascript
function create (o) {
    function fn () {};
    fn.prototype = o;
    return new fn();
}
```
