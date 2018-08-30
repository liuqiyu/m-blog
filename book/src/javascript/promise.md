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
// 判断变量否为function
const isFunction = variable => typeof variable === 'function'
// 定义Promise的三种状态常量
const PENDING = 'PENDING'
const FULFILLED = 'FULFILLED'
const REJECTED = 'REJECTED'

class MyPromise {
  constructor (handle) {
    if (!isFunction(handle)) {
      throw new Error('MyPromise must accept a function as a parameter')
    }
    // 添加状态
    this._status = PENDING
    // 添加状态
    this._value = undefined
    // 添加成功回调函数队列
    this._fulfilledQueues = []
    // 添加失败回调函数队列
    this._rejectedQueues = []
    // 执行handle
    try {
      handle(this._resolve.bind(this), this._reject.bind(this)) 
    } catch (err) {
      this._reject(err)
    }
  }
  // 添加resovle时执行的函数
  _resolve (val) {
    if (this._status !== PENDING) return
    this._status = FULFILLED
    this._value = val
    const callback = () => {
      // 依次执行队列中的函数，并清空队列
      const runFulfilled = (value) => {
        let cb;
        while (cb = this._fulfilledQueues.shift()) {
          cb(value)
        }
      }
      const runRejected = (value) => {
        let cb;
        while (cb = this._rejectedQueues.shift()) {
          cb(value)
        }
      }
      if (val instanceof MyPromise) {
        val.then(runFulfilled, runRejected)
      } else {
        runFulfilled(val)
      }
    }
    // 为了支持同步的Promise
    setTimeout(callback, 0)
  }
  // 添加reject时执行的函数
  _reject (err) { 
    if (this._status !== PENDING) return
    this._status = REJECTED
    this._value = err
    // 依次执行失败队列中的函数，并清空队列
    const run = () => {
      let cb;
      while (cb = this._rejectedQueues.shift()) {
        cb(err)
      }
    }
    // 为了支持同步的Promise
    setTimeout(run, 0)
  }
  // 添加then方法
  then (onFulfilled, onRejected) {
    const { _value, _status } = this
    // 返回一个新的Promise对象
    return new MyPromise((onFulfilledNext, onRejectedNext) => {
      // 封装一个成功时执行的函数
      let fulfilled = value => {
        try {
          if (!isFunction(onFulfilled)) {
            onFulfilledNext(value)
          } else {
            let res =  onFulfilled(value);
            if (res instanceof MyPromise) {
              // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
              res.then(onFulfilledNext, onRejectedNext)
            } else {
              //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
              onFulfilledNext(res)
            }
          }
        } catch (err) {
          // 如果函数执行出错，新的Promise对象的状态为失败
          onRejectedNext(err)
        }
      }
      // 封装一个失败时执行的函数
      let rejected = error => {
        try {
          if (!isFunction(onRejected)) {
            onRejectedNext(error)
          } else {
              let res = onRejected(error);
              if (res instanceof MyPromise) {
                // 如果当前回调函数返回MyPromise对象，必须等待其状态改变后在执行下一个回调
                res.then(onFulfilledNext, onRejectedNext)
              } else {
                //否则会将返回结果直接作为参数，传入下一个then的回调函数，并立即执行下一个then的回调函数
                onFulfilledNext(res)
              }
          }
        } catch (err) {
          // 如果函数执行出错，新的Promise对象的状态为失败
          onRejectedNext(err)
        }
      }
      switch (_status) {
        // 当状态为pending时，将then方法回调函数加入执行队列等待执行
        case PENDING:
          this._fulfilledQueues.push(fulfilled)
          this._rejectedQueues.push(rejected)
          break
        // 当状态已经改变时，立即执行对应的回调函数
        case FULFILLED:
          fulfilled(_value)
          break
        case REJECTED:
          rejected(_value)
          break
      }
    })
  }
  catch (onRejected) {
    return this.then(undefined, onRejected)
  }
  // 添加静态resolve方法
  static resolve (value) {
    // 如果参数是MyPromise实例，直接返回这个实例
    if (value instanceof MyPromise) return value
    return new MyPromise(resolve => resolve(value))
  }
  // 添加静态reject方法
  static reject (value) {
    return new MyPromise((resolve ,reject) => reject(value))
  }
  // 添加静态all方法
  static all (list) {
    return new MyPromise((resolve, reject) => {
      /**
       * 返回值的集合
       */
      let values = []
      let count = 0
      for (let [i, p] of list.entries()) {
        // 数组参数如果不是MyPromise实例，先调用MyPromise.resolve
        this.resolve(p).then(res => {
          values[i] = res
          count++
          // 所有状态都变成fulfilled时返回的MyPromise状态就变成fulfilled
          if (count === list.length) resolve(values)
        }, err => {
          // 有一个被rejected时返回的MyPromise状态就变成rejected
          reject(err)
        })
      }
    })
  }
  // 添加静态race方法
  static race (list) {
    return new MyPromise((resolve, reject) => {
      for (let p of list) {
        // 只要有一个实例率先改变状态，新的MyPromise的状态就跟着改变
        this.resolve(p).then(res => {
          resolve(res)
        }, err => {
          reject(err)
        })
      }
    })
  }
  finally (cb) {
    return this.then(
      value  => MyPromise.resolve(cb()).then(() => value),
      reason => MyPromise.resolve(cb()).then(() => { throw reason })
    );
  }
}
```

`handle` 对应的是一个回调函数：

```javascript
(resolve, reject) => {
       resolve(value); // 成功
        reject(value);  // 失败
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

