# JavaScript Garden

## 对象

> 1、JavaScript 中所有变量都可以当作对象使用，除了两个例外 null 和 undefined。 
 
> 2、点操作符或者中括号操作符，都可以用来访问对象的属性；
> 但是中括号操作符在下面两种情况下依然有效：
> + 动态设置属性
> + 属性名不是一个有效的变量名（译者注：比如属性名中包含空格，或者属性名是 JS 的关键词）  

```javascript
var foo = {name: 'kitten'}
foo.name; // kitten
foo['name']; // kitten

var get = 'name';
foo[get]; // kitten

foo.1234; // SyntaxError
foo['1234']; // works
```

> 3、JavaScript 不包含传统的类继承模型，而是使用 prototype 原型模型。    JavaScript 使用原型链的继承方式。

> 4、试图获取一个不存在的属性将会遍历整个原型链。并且，当使用 for in 循环遍历对象的属性时，原型链上的所有属性都将被访问。

> 5、为了判断一个对象是否包含自定义属性而不是原型链上的属性， 我们需要使用继承自 Object.prototype 的 hasOwnProperty 方法。
注意: 通过判断一个属性是否 undefined 是不够的。 因为一个属性可能确实存在，只不过它的值被设置为 undefined。
hasOwnProperty 是 JavaScript 中唯一一个处理属性但是不查找原型链的函数。

## 函数

> 1、函数同变量一样，也会有提升。（由于赋值语句只在运行时执行）

> 2、函数 是 JavaScript 中唯一拥有自身作用域的结构

> 3、JavaScript 中每个函数内都能访问一个特别变量 arguments。这个变量维护着所有传递到这个函数中的参数列表。

> 4、通过 new 关键字方式调用的函数都被认为是构造函数。在构造函数内部 - 也就是被调用的函数内 - this 指向新创建的对象 Object。 这个新创建的对象的 prototype 被指向到构造函数的 prototype

> 5、由于 for in 循环会枚举原型链上的所有属性，唯一过滤这些属性的方式是使用 hasOwnProperty 函数， 因此会比普通的 for 循环慢上好多倍。

## 核心

> 1、永远不要使用eval函数

> 2、下面的情况会返回 undefined 值：
> * 访问未修改的全局变量 undefined。
> - 由于没有定义 return 表达式的函数隐式返回。
> * return 表达式没有显式的返回任何内容。
> - 访问不存在的属性。
> * 函数参数没有被显式的传递值。    
> + 任何被设置为 undefined 值的变量。

## 其他

> 1、setTimeout 和 setInterval 定时处理不是 ECMAScript 的标准，它们在 DOM (文档对象模型) 被实现。

> 2、setTimeout 和 setInterval 也接受第一个参数为字符串的情况。 这个特性绝对不要使用，因为它在内部使用了 eval。

