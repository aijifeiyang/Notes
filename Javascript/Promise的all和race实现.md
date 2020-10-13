## all实现

```javascript
Promise.myAll = function (arr) {
    return new Promise ((resolve, reject) => {
        let res = [];
        let index = 0;
        for (let i = 0, len = arr.length; i < len; i++) {
            arr[i].then(data => {
                res[i] = data;
                if (++index === len) resolve(res);
            }, reject);
        }
    });
}
```

## race实现

```javascript
Promise.myRace = function (arr) {
    return new Promise((resolve, reject) => {
        for (let i = 0, len = arr.length; i < len; i++) {
            arr[i].then(resolve, reject);
        }
    })
}
```
