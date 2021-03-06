面试官：**谈谈你对 JS 原型和原型链的理解？**    

候选人：JS 原型是指为其它对象提供共享属性访问的对象。在创建对象时，每个对象都包含一个隐式引用指向它的原型对象或者 null。    
&nbsp;原型也是对象，因此它也有自己的原型。这样构成一个原型链。


面试官：**原型链有什么作用？**

候选人：在访问一个对象的属性时，实际上是在查询原型链。这个对象是原型链的第一个元素，先检查它是否包含属性名，如果包含则返回属性值，否则检查原型链上的第二个元素，以此类推。    


面试官：**那如何实现原型继承呢？**

候选人：有两种方式。一种是通过 Object.create 或者 Object.setPrototypeOf 显式继承另一个对象，将它设置为原型。       
        另一种是通过 constructor 构造函数，在使用 new 关键字实例化时，会自动继承 constructor 的 prototype 对象，作为实例的原型。       
        在 ES2015 中提供了 class 的风格，背后跟 constructor 工作方式一样，写起来更内聚一些。                


面试官：**ConstructorB 如何继承 ConstructorA ？**

候选人：JS 里的继承，是对象跟对象之间的继承。constructor 的主要用途是初始化对象的属性。             
        因此，两个 Constructor 之间的继承，需要分开两个步骤。                       
        第一步是，编写新的 constructor，将两个 constructor 通过 call/apply 的方式，合并它们的属性初始化。按照超类优先的顺序进行。         
        第二步是，取出超类和子类的原型对象，通过 Object.create/Object.setPrototypeOf 显式原型继承的方式，设置子类的原型为超类原型。                
        整个过程手动编写起来比较繁琐，因此建议通过 ES2015 提供的 **class** 和 **extends** 关键字去完成继承，它们内置了上述两个步骤。          


面试官：**看起来你挺了解原型，你能说一个原型里比较少人知道的特性吗？**

候选人：在 ES3 时代，只有访问属性的 get 操作能触发对原型链的查找。在 ES5 时代，新增了 accessor property 访问器属性的概念。它可以定义属性的 getter/setter 操作。        
        具有访问器属性 setter 操作的对象，作为另一个对象的原型的时候，设置属性的 set 操作，也能触发对原型链的查找。    
        普通对象的 __proto__ 属性，其实就是在原型链查找出来的，它定义在 Object.prototype 对象上。

