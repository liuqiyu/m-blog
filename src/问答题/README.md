# 问答题

### 1、请解释一下Javascript的同源策略

```
同源策略是指： 协议、域名、端口都相同。同源策略是一种安全协议，一段脚本只能执行统一来源的属性。

为什么要同源策略：
加入银行系统的一个登陆表单，黑客利用iframe内嵌到他的页面里面，就可以在用户点击提交表单时候，获取到用户名与密码。
```

### 2、GET和POST的区别？

```
GET: 一般用于信息的获取，使用URL传递参数，但是对所发的参数数量也会有现在，一般在2000个字符左右。
POST：一般用于修改服务器资源，对所发的信息没有限制，通过提交表单传值。
```

### 3、eval是做什么的？

```
1、将字符串解析成js代码并运行。
2、应该避免使用eval，不安全，非常耗性能
```

### 4、node.js适合的场景

```
1、高并发
2、聊天
3、实时消息推送
```

#### 5、解释一下原型和原型链？（自我发挥）

```
原型： 每一个函数都会自带原型属性
原型链： 每个构造函数的原型对象都有一个constructor参数指向构造函数本身，每一个函数的实例都会有一个隐形原型__proto__（执政）指向构造函数的原型。这些链式的关系就是原型链
```

#### 6、事件捕获  事件冒泡
    * this与e.target的区别： this会向上冒泡，但是e.target只会指向触发事件的dom。

#### 7、清除浮动

```
1、在浮动元素后面加一个添加 clear: both; 的属性。
2、给浮动的容易上添加overflow: hidden;或者overflow: auto;
3、给浮动的容器也加上浮动（不考虑）
4、使用css的：after伪元素。
    .clearfix:after {
        content: '.';
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
    }
    .clearfix {
        zoom: 1;
    }
```

#### 8、null NaN undefined

```
null: Null类型，代表空值，一个空对象的指针。typeof null ==> "object"

NaN: 保留字（表明数据类型不是数字）。 isNaN  => true  (该值不是数字)
    * 不能通过 == 或者 === 来判断。
    * 0/0会返回NaN
    * 如果isNaN函数的参数不是Number类型。isNaN函数会首先尝试将它转化为数值。然后才会对转化后的结果进行是否NaN的判断。
        所以会出现下面情况;
        * isNaN('12');  // false： 可以先转化为12在进行判断
        * 

undefined: 缺省值
```

#### 9、列举3个css3新特性、列举3个HTML5新特性、3个ES6新特性

```
// css3
1、border-radius：圆角边框
2、media query 媒体查询
3、@keyframes

// html5
1、header
2、section
3、footer

// es6
1、let const 
2、解构
3、promise
4、class
```

#### 10、两种方案，实现div以每秒50px的速度左移100px

```
1、css3
div {
    trabstion: all 2s linear;
}

div.style.left = (div.offsetLeft + 100) + 'px';

2、jquery
$(element).animate({ 'left': 10 }, 2000)
```

#### 11、实现不确定宽高元素水平垂直居中

```
1、
{
    display: flex;
    justify-content: center;
    aligin-items: center;
}

2、 
{
    transform: translate(-50%, -50%);
    position: relative;
    top: 50%
    left: 50%;
}
```

#### 12、js兼容性问题举例

```
1、ie8以下的点击事件： attachevent。 其他： addEventListener
2、ie8以下清除冒泡： e.returnValue = false。  其他：  e.stopPropagation();
```

#### 13、前端优化手段

```
1、较少http请求，css sprites。
2、使用gzip
3、使用cdn，外部的js和css脚本。
4、样式表放在头部，js标本放在底部
5、避免重定向
```

#### 14、变量函数提升

* 函数表达式(var a =  function(){})会覆盖声明式函数(function a(){})

```
console.log(a);
var a = 1;
function a(){};

==>

var a;
function a(){}
console.log(a);
a = 1;

// function a(){}
```

#### 15、行内元素与块级元素

```
块级元素： div p form ul ol li

行内元素： span em i font 

* 块级元素可以设置margin和padding，行内元素水平方向的margin和padding。（margin-left、padding-right）可以产生效果。但是竖直方向的（padding-top、margin-bottom）不会生效。
```

#### 16、置换元素

```
1、一个 内容 不受CSS视觉格式化模型控制，CSS渲染模型并不考虑对此内容的渲染，且元素本身一般拥有固有尺寸（宽度，高度，宽高比）的元素，被称之为置换元素。
2、替换元素是浏览器根据元素的标签和属性，觉得元素具体显示什么内容
3、img、input、select等
```

#### 17、RESTful API? 面向资源

```
URL定位资源，用HTTP动词（GET, POST, PUT, DELETE）描述操作。

1、资源：JSON实现现在最常用的资源表现形式.
2、统一接口：CRUD分别对应HTTP方法：
    * POST - 新建资源
    * GET - 获取资源
    * PUT - 更新资源
    * DELETE - 删除资源
3、无状态。
    有状态：加入获取一个人银行工资需要有一下步骤：
        ①、登录银行
        ②、进入查询工资的页面
        ③、搜索改员工
        ④、点击查看工资
    无状态：直接通过一个URL获取工资。 http://www.getMoney.com/v1?userId=12
```

#### 18、表现与数据分离。mvvm框架

#### 19、JS将类数组转换成数组的方式

```javascript
var arrayLike = {
    '0' : 'a',
    '1' : 'b',
    '2' : 'c',
    length : 3
};

let arr1 = Array.from(arrayLike);
let arr2 = Array.prototype.slice.call(arrayLike);
let arr3 = [].slice.call(arrayLike);
```
