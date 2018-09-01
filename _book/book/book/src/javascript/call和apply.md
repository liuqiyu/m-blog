# call和apply

> 通俗易懂: 就是将`obj1`的`this`指向`obj2`。

### 1、call()

* 语法：`obj1.call(obj2[, param1, param2, ...])`
* 定义：用obj2对象来代替obj1，调用obj1的方法。
* 说明：call 方法可以用来代替另一个对象调用一个方法。
call 方法可将一个函数的对象上下文从初始的上下文改变为由 obj2 指定的新对象。 如果没有提供 obj2参数，那么 Global 对象被用作 obj2。 

### 2、apply()

* 语法： `obj1.apply(obj2[, array])`
* 定义： 用obj2对象来代替obj1，调用obj1的方法。
* 说明：call ()和apply()作用一样，但是call()可以接收任何类型的参数，而apply()只能接收数组参数。

```javascript
function fruits() {}
 
fruits.prototype = {
    color: "red",
    say: function() {
        console.log("My color is " + this.color);
    }
}
 
var apple = new fruits;
apple.say();    //My color is red

banana = {
    color: "yellow"
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow
```