## Object.create 实现
```javascript
function create1013 (o) {
    function fn () {};
    fn.prototype = o;
    return new fn();
}
```

## Object.assign 实现

```javascript
function assign1016 (target, ...source) {
    return source.reduce((cur, next) => {
        if (typeof cur !== 'function' && typeof cur !== 'object' || cur === null) cur = new Object(acc);
        if (next === null) return cur;
        [...Object.keys(next), ...Object.getOwnPropertySymbols(next)].forEach(key => {
            cur[key] = next[key];
        })
        return cur;
    }, target);
}
```
