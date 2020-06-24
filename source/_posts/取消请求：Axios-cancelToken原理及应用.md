---
title: 取消请求：Axios cancelToken原理及应用
date: 2020-06-24 14:18:44
tags:
---
&emsp;前一段时间在做后台管理系统上传图片时遇到这么一个问题：上传图片相关操作都在一个element-ui的dialog对话框内完成，若在图片上传完成之前，用户点击"取消"按钮关闭了对话框，此时处于pending状态的上传图片请求需要取消。

&emsp;为了解决这个问题，后来就跑去查了一下Axios API文档，发现里面有一段对cancelToken取消请求的介绍，挺有意思的，记录如下。
### 基本使用
我们先来看看CancelToken的基本用法：
#### 第一种方式
``` JavaScript
const CancelToken = axios.CancelToken;
const source = CancelToken.source();

axios.get('/user/12345', {
  cancelToken: source.token
}).catch(function (thrown) {
  if (axios.isCancel(thrown)) {
    // 请求主动被取消
    console.log('请求已被取消', thrown.message);
  } else {
    // 处理其他错误
  }
});

axios.post('/user/12345', {
  name: 'new name'
}, {
  cancelToken: source.token
})

// 取消请求 (消息参数是可选的)
source.cancel('不想请求了');
```
注意，虽然例子里没有列出来，但是含有cancelToken键的对象参数在:
1. 发送delete，head和options请求时，是放在第`二`个参数里
2. 发送post, put, patch请求时，是放在第`三`个参数里

#### 第二种方式
直接调用执行器里里的cancel方法
``` JavaScript
const CancelToken = axios.CancelToken;
let cancel;

axios.get('/user/12345', {
  cancelToken: new CancelToken(function executor(c) {
    cancel = c
  })
})

cancel()
```
Ok, 那么好奇~~(作死)~~的小伙伴看到就会问了啊
>为什么它这么写就可以取消请求啊？

>它内部到底是如何实现的？

PS：也是以防面试时被问到了答不上来~(●'◡'●)

### 源码分析
以下摘自axios v0.19.0: `node_modules/axios/lib/adapters/xhr.js` 159行至171行
``` JavaScript
    if (config.cancelToken) {
      // Handle cancellation
      config.cancelToken.promise.then(function onCanceled(cancel) {
        if (!request) {
          return;
        }

        request.abort();
        reject(cancel);
        // Clean up request
        request = null;
      });
    }
```
分析：从上述源码可以看到，发送请求过程中，如果config对象参数里有cancelToken键，执行axios.CancelToken.source().token原型的promise方法

以下摘自axios v0.19.0: `node_modules/axios/lib/cancel/CancelToken.js`
``` JavaScript
/**
 * Returns an object that contains a new `CancelToken` and a function that, when called,
 * cancels the `CancelToken`.
 */
CancelToken.source = function source() {
  var cancel;
  var token = new CancelToken(function executor(c) {
    cancel = c;
  });
  return {
    token: token,
    cancel: cancel
  };
};
```
分析：可以看到，当用户调用source.cancel()方法的时候，实际上调用了传入executor函数的第一个参数函数cancel，上面第二种用法就是这样使用的

下面我们来分析一下CancelToken类
``` JavaScript
'use strict';

var Cancel = require('./Cancel');

/**
 * A `CancelToken` is an object that can be used to request cancellation of an operation.
 *
 * @class
 * @param {Function} executor The executor function.
 */
function CancelToken(executor) {
  if (typeof executor !== 'function') {
    throw new TypeError('executor must be a function.');
  }

  var resolvePromise;
  this.promise = new Promise(function promiseExecutor(resolve) {
    resolvePromise = resolve;
  });

  var token = this;
  // 执行了里面的cancel方法
  executor(function cancel(message) {
    if (token.reason) {
      // Cancellation has already been requested
      return;
    }

    token.reason = new Cancel(message);
    resolvePromise(token.reason);
  });
}
```
分析：记得`xhr.js`里的`config.cancelToken.promise.then()`函数吗？这个promise的resolve方法会在cancel方法被调用之后触发，最后调用到request.abort()中止请求~