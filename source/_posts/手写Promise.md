---
title: 手写Promise
categories: javascript
tags: javascript
keywords: [promise]
description: 使用传统的方法实现原生Promise类
---
# 手写Promise

前几天在掘金上看到一个大佬写的一个手写promise的教程，然后自己也实现了一下，做下梳理

首先Promise是一个类，为了和原生的Promise区分开，这里我们改名叫做MyPromise,以下就是具体的实现过程,
> 我只写了一些基本的功能，一些容错的处理没有写(等有时间补上)

```javascript
class MyPromise {
  constructor(executor) {
    // 存储状态
    this.state = 'pending';
    // 存储成功的值
    this.value = '';
    // 存储失败的值
    this.reason = '';
    // 存储成功的回调
    this.onResolvedCallbacks = [];
    // 存储失败的回调
    this.onRejectedCallbacks = [];
    // resolve 方法要做事在这里
    let resolve = value => {
      this.state = 'fulfilled';
      this.value = value;
      this.onResolvedCallbacks.forEach(resolveFn => resolveFn(value));
    }
    // reject 方法要做事在这里
    let reject = reason => {
      this.state = 'rejected';
      this.reason = reason;
      this.onRejectedCallbacks.forEach(rejectFn => rejectFn(reason));
    }
    // 实例化的时候执行
    executor(resolve, reject);
  }
  // 实现方法异步
  static async (fn) {
    setTimeout(() => {
      fn()
    }, 0);
  }
  /**
   * then 方法是promise运行之后的结果在这里面获取，并且它返回的是一个新的promise
   * @param {*} onFulfilledFn 
   * @param {*} onRejectedFn 
   */
  then(onFulfilledFn, onRejectedFn) {
    // 判断then方法里面的resolve方法和reject方法是否存在，并且是否为函数
    if (!onFulfilledFn || typeof onFulfilledFn !== 'function') {
      onFulfilledFn = function () {};
    }
    if (!onRejectedFn || typeof onRejectedFn !== 'function') {
      onRejectedFn = function () {};
    }
    // then方法返回的是一个新的promise
    let promise1 = new MyPromise((resolve, reject) => {
      if (this.state === 'fulfilled') {
        MyPromise.async(() => {
          let value = onFulfilledFn(this.value);
          if (value && promise1 !== value) resolve(value);
        });
      }
      if (this.state === 'rejected') {
        MyPromise.async(() => {
          let reason = onRejectedFn(this.reason);
          if (reason && promise1 !== reason) reject(reason);
        });
      }
      if (this.state === 'pending') {
        this.onResolvedCallbacks.push(() => {
          MyPromise.async(() => {
            onFulfilledFn(this.value);
          })
        });
        this.onRejectedCallbacks.push(() => {
          MyPromise.async(() => {
            onRejectedFn(this.reason);
          })
        });
      }
    })
    return promise1;
  }

  catch (onRejectedFn) {
    let newPromise = new MyPromise((resolve, reject) => {
      if (this.state === 'rejected') {
        let reason = onRejectedFn(this.reason);
        if (reason) resolve(reason);
      }
    })
    return newPromise;
  }

  static race(iterable) {
    let result = [];
    return new MyPromise((resolve, reject) => {
      for (let i = 0; i < iterable.length; i++) {
        iterable[i].then(res => {
          if (!result.length) {
            result.push(res)
            resolve(res);
          };
        }, rea => {
          if (!result.length) {
            result.push(rea)
            reject(rea);
          };
        });
      }
    })
  }

  static all(iterable) {
    let result = [];
    return new MyPromise((resolve, reject) => {
      for (let i = 0; i < iterable.length; i++) {
        iterable[i].then(res => {
          let len = result.push(res);
          if (len === iterable.length) {
            resolve(result);
          }
        }, rea => {
          reject(rea);
        });
      }
    });
  }

  static resolve(value) {
    return new MyPromise((resolve, reject) => {
      resolve(value)
    });
  }
  
  static reject(reason) {
    return new MyPromise((resolve, reject) => {
      reject(reason)
    });
  }
}


// 测试代码
let p1 = new MyPromise(function (resolve, reject) {
  setTimeout(() => {
    resolve(1)
  }, 1000);
});
let p2 = new MyPromise((resolve, reject) => {
  reject(2)
});
p2.catch(rea => {
  console.log('catch', rea)
  return 3;
}).then(res => {
  console.log('then-res', res);
})
p2.then(null, rea => {
  console.log('then-rea', rea)
})
MyPromise.race([p1, p2]).then(res => {
  console.log(res);
})
MyPromise.all([p1, p2]).then(res => {
  console.log('all', res);
})
p2.then(null, r => {
  console.log('cuole', r)
})
```
