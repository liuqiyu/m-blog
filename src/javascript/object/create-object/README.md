# 创建对象的方法

## 目录

* [工厂模式](#create1)
* [构造函数模式](#create2)
* [原型模式](#create3)
* [工厂模式](#create1)
* [工厂模式](#create1)

<a name="create1"></a>
#### 一、工厂模式

```javascript
function createPerson(name, age) {
    var o = new Object();
    o.name = name;
    o.age = age;
    o.say = function () {
        alert(this.name);
    }
}

var person1 = createPerson('xuan', 18);
var person2 = createPerson('kaway', 18);
```

<a name="create2"></a>
#### 二、构造函数模式

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.say = function() {
         alert(this.name);
    }
}

var per1 = new Person('xuan', 18);
var per2 = new Person('kaway', 18);
```
构造函数首字母大写，非构造函数首字母小写

- 判断题：
* 1、 per1.constructor === Person  // true
* 2、 per2 instanceof Person       // true
* 3、 per2 instanceof Object       // true
* 4、 Person instanceof Object     // true

- 弊端： 每次实例化会重新去创建属性和方法。可以将方法指向外部的一个函数指针，或者使用原型模式。

<a name="create3"></a>
#### 三、原型模式
我们每次创建函数都会有一个`prototype`属性，这个属性是一个指针，指向一个对象。

好处： 可以让所有对象实例共享它包含的属性和方法。

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.say = function() {
    alert(this.name);
};

var per1 = new Person('xuan', 18);
var per2 = new Person('kaway', 18);
```