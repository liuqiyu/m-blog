# Promise

## 一、实现原理

#### 1、基本结构

```javascript
new Promise((resolve, reject) => {
    resolve(value); // 成功
    reject(value);  // 失败
});
```

#### 2、代码

```javascript
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

class MyPromise {
    constructor(handle) {
        if (!isFunction(handle)) {
            throw new Error('MyPromise 必须接收一个函数作为参数！');
        }
        this._status = PENDING;
        this._value = undefined;
        try {
            handle(this._resolve.bind(this), this._reject.bind(this));
        } catch(err) {
            this._reject(err);
        }
    }
    
    // 添加reslove时执行的函数
    _resolve(val) {
        if (this._status !== PENFING) return;
        this._status = FULFILLED; // 成功
        this._value = val;
    }
    
    // 添加reject时执行的函数
    __reject(err) {
        if (this._status !== PENDING) return;
        this._status = REJECTED;
        this._value = err;
    }
}
```

#### 3、方法

* then().    //  实例添加状态改变时的回调函数
* catch().   //  发生错误的回调函数
* finally(). //  不管 Promise 对象最后状态如何，都会执行的操作
* all().     //  将多个Promise实例，包装成一个新的`Promise`实例    ·

<hr/>

## 二、基本用法

**三种状态：**

* `pending`： 进行中
* `fulfilled`： 已成功（resolved）
* `rejected`： 已失败

#### 1、简单例子
```javascript
const promise = new Promise((resolve, reject) => {
    if (success) {
      resolve(value);
    } else {
      reject(error);  
    }
});

promise.then((res) => {
    
}, (error) => {
    
});
```

#### 2、传参

```javascript
function promise(name) {
    return new Promise((resolve,  reject) => {
        if (success) {
              resolve(value);
            } else {
              reject(error);  
            }
    });
}

promise('kaway').then((res) => {
    console.log(value);
});
```

