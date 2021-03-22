# Es6_Promise

+ Es6_Promise 实现 遵循Es6_Promise/A+规范
+ [Es6_Promise/A+规范译文](https://malcolmyu.github.io/2015/06/12/Es6_Promises-A-Plus/#note-4)


``` js

// Es6_Promise 三个状态
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";

function Es6_Promise(excutor) {
    console.log('/执行/')
    let that = this; // 缓存当前Es6_Promise实例对象
    that.status = PENDING; // 初始状态
    that.value = undefined; // fulfilled状态时 返回的信息
    that.reason = undefined; // rejected状态时 拒绝的原因
    that.onFulfilledCallbacks = []; // 存储fulfilled状态对应的onFulfilled函数
    that.onRejectedCallbacks = []; // 存储rejected状态对应的onRejected函数

    function resolve(value) { // value成功态时接收的终值
        if(value instanceof Es6_Promise) {
            return value.then(resolve, reject);
        }

        // 为什么resolve 加setTimeout?
        // 2.2.4规范 onFulfilled 和 onRejected 只允许在 execution context 栈仅包含平台代码时运行.
        // 注1 这里的平台代码指的是引擎、环境以及 Es6_Promise 的实施代码。实践中要确保 onFulfilled 和 onRejected 方法异步执行，且应该在 then 方法被调用的那一轮事件循环之后的新执行栈中执行。

        setTimeout(() => {
            // 调用resolve 回调对应onFulfilled函数
            if (that.status === PENDING) {
                // 只能由pedning状态 => fulfilled状态 (避免调用多次resolve reject)
                that.status = FULFILLED;
                that.value = value;
                that.onFulfilledCallbacks.forEach(cb => cb(that.value));
            }
        });
    }

    function reject(reason) { // reason失败态时接收的拒因
        setTimeout(() => {
            // 调用reject 回调对应onRejected函数
            if (that.status === PENDING) {
                // 只能由pedning状态 => rejected状态 (避免调用多次resolve reject)
                that.status = REJECTED;
                that.reason = reason;
                that.onRejectedCallbacks.forEach(cb => cb(that.reason));
            }
        });
    }

    // 捕获在excutor执行器中抛出的异常
    // new Es6_Promise((resolve, reject) => {
    //     throw new Error('error in excutor')
    // })
    try {
        excutor(resolve, reject);
    } catch (e) {
        reject(e);
    }
}

/**
 * resolve中的值几种情况：
 * 1.普通值
 * 2.Es6_Promise对象
 * 3.thenable对象/函数
 */

/**
 * 对resolve 进行改造增强 针对resolve中不同值情况 进行处理
 * @param  {Es6_Promise} Es6_Promise2 Es6_Promise1.then方法返回的新的Es6_Promise对象
 * @param  {[type]} x         Es6_Promise1中onFulfilled的返回值
 * @param  {[type]} resolve   Es6_Promise2的resolve方法
 * @param  {[type]} reject    Es6_Promise2的reject方法
 */
function resolveEs6_Promise(Es6_Promise2, x, resolve, reject) {
    if (Es6_Promise2 === x) {  // 如果从onFulfilled中返回的x 就是Es6_Promise2 就会导致循环引用报错
        return reject(new TypeError('循环引用'));
    }

    let called = false; // 避免多次调用
    // 如果x是一个Es6_Promise对象 （该判断和下面 判断是不是thenable对象重复 所以可有可无）
    if (x instanceof Es6_Promise) { // 获得它的终值 继续resolve
        if (x.status === PENDING) { // 如果为等待态需等待直至 x 被执行或拒绝 并解析y值
            x.then(y => {
                resolveEs6_Promise(Es6_Promise2, y, resolve, reject);
            }, reason => {
                reject(reason);
            });
        } else { // 如果 x 已经处于执行态/拒绝态(值已经被解析为普通值)，用相同的值执行传递下去 Es6_Promise
            x.then(resolve, reject);
        }
        // 如果 x 为对象或者函数
    } else if (x != null && ((typeof x === 'object') || (typeof x === 'function'))) {
        try { // 是否是thenable对象（具有then方法的对象/函数）
            let then = x.then;
            if (typeof then === 'function') {
                then.call(x, y => {
                    if(called) return;
                    called = true;
                    resolveEs6_Promise(Es6_Promise2, y, resolve, reject);
                }, reason => {
                    if(called) return;
                    called = true;
                    reject(reason);
                })
            } else { // 说明是一个普通对象/函数
                resolve(x);
            }
        } catch(e) {
            if(called) return;
            called = true;
            reject(e);
        }
    } else {
        resolve(x);
    }
}

/**
 * [注册fulfilled状态/rejected状态对应的回调函数]
 * @param  {function} onFulfilled fulfilled状态时 执行的函数
 * @param  {function} onRejected  rejected状态时 执行的函数
 * @return {function} newPromsie  返回一个新的Es6_Promise对象
 */
Es6_Promise.prototype.then = function(onFulfilled, onRejected) {
    const that = this;
    let newEs6_Promise;
    // 处理参数默认值 保证参数后续能够继续执行
    onFulfilled =
        typeof onFulfilled === "function" ? onFulfilled : value => value;
    onRejected =
        typeof onRejected === "function" ? onRejected : reason => {
            throw reason;
        };

    // then里面的FULFILLED/REJECTED状态时 为什么要加setTimeout ?
    // 原因:
    // 其一 2.2.4规范 要确保 onFulfilled 和 onRejected 方法异步执行(且应该在 then 方法被调用的那一轮事件循环之后的新执行栈中执行) 所以要在resolve里加上setTimeout
    // 其二 2.2.6规范 对于一个Es6_Promise，它的then方法可以调用多次.（当在其他程序中多次调用同一个Es6_Promise的then时 由于之前状态已经为FULFILLED/REJECTED状态，则会走的下面逻辑),所以要确保为FULFILLED/REJECTED状态后 也要异步执行onFulfilled/onRejected

    // 其二 2.2.6规范 也是resolve函数里加setTimeout的原因
    // 总之都是 让then方法异步执行 也就是确保onFulfilled/onRejected异步执行

    // 如下面这种情景 多次调用p1.then
    // p1.then((value) => { // 此时p1.status 由pedding状态 => fulfilled状态
    //     console.log(value); // resolve
    //     // console.log(p1.status); // fulfilled
    //     p1.then(value => { // 再次p1.then 这时已经为fulfilled状态 走的是fulfilled状态判断里的逻辑 所以我们也要确保判断里面onFuilled异步执行
    //         console.log(value); // 'resolve'
    //     });
    //     console.log('当前执行栈中同步代码');
    // })
    // console.log('全局执行栈中同步代码');
    //

    if (that.status === FULFILLED) { // 成功态
        return newEs6_Promise = new Es6_Promise((resolve, reject) => {
            setTimeout(() => {
                try{
                    let x = onFulfilled(that.value);
                    resolveEs6_Promise(newEs6_Promise, x, resolve, reject); // 新的Es6_Promise resolve 上一个onFulfilled的返回值
                } catch(e) {
                    reject(e); // 捕获前面onFulfilled中抛出的异常 then(onFulfilled, onRejected);
                }
            });
        })
    }

    if (that.status === REJECTED) { // 失败态
        return newEs6_Promise = new Es6_Promise((resolve, reject) => {
            setTimeout(() => {
                try {
                    let x = onRejected(that.reason);
                    resolveEs6_Promise(newEs6_Promise, x, resolve, reject);
                } catch(e) {
                    reject(e);
                }
            });
        });
    }

    if (that.status === PENDING) { // 等待态
        // 当异步调用resolve/rejected时 将onFulfilled/onRejected收集暂存到集合中
        return newEs6_Promise = new Es6_Promise((resolve, reject) => {
            that.onFulfilledCallbacks.push((value) => {
                try {
                    let x = onFulfilled(value);
                    resolveEs6_Promise(newEs6_Promise, x, resolve, reject);
                } catch(e) {
                    reject(e);
                }
            });
            that.onRejectedCallbacks.push((reason) => {
                try {
                    let x = onRejected(reason);
                    resolveEs6_Promise(newEs6_Promise, x, resolve, reject);
                } catch(e) {
                    reject(e);
                }
            });
        });
    }
};

/**
 * Es6_Promise.all Es6_Promise进行并行处理
 * 参数: Es6_Promise对象组成的数组作为参数
 * 返回值: 返回一个Es6_Promise实例
 * 当这个数组里的所有Es6_Promise对象全部变为resolve状态的时候，才会resolve。
 */
Es6_Promise.all = function(Es6_Promises) {
    return new Es6_Promise((resolve, reject) => {
        let done = gen(Es6_Promises.length, resolve);
        Es6_Promises.forEach((Es6_Promise, index) => {
            Es6_Promise.then((value) => {
                done(index, value)
            }, reject)
        })
    })
}

function gen(length, resolve) {
    let count = 0;
    let values = [];
    return function(i, value) {
        values[i] = value;
        if (++count === length) {
            console.log(values);
            resolve(values);
        }
    }
}

/**
 * Es6_Promise.race
 * 参数: 接收 Es6_Promise对象组成的数组作为参数
 * 返回值: 返回一个Es6_Promise实例
 * 只要有一个Es6_Promise对象进入 FulFilled 或者 Rejected 状态的话，就会继续进行后面的处理(取决于哪一个更快)
 */
Es6_Promise.race = function(Es6_Promises) {
    return new Es6_Promise((resolve, reject) => {
        Es6_Promises.forEach((Es6_Promise, index) => {
           Es6_Promise.then(resolve, reject);
        });
    });
}

// 用于Es6_Promise方法链时 捕获前面onFulfilled/onRejected抛出的异常
Es6_Promise.prototype.catch = function(onRejected) {
    return this.then(null, onRejected);
}

Es6_Promise.resolve = function (value) {
    return new Es6_Promise(resolve => {
        resolve(value);
    });
}

Es6_Promise.reject = function (reason) {
    return new Es6_Promise((resolve, reject) => {
        reject(reason);
    });
}

/**
 * 基于Es6_Promise实现Deferred的
 * Deferred和Es6_Promise的关系
 * - Deferred 拥有 Es6_Promise
 * - Deferred 具备对 Es6_Promise的状态进行操作的特权方法（resolve reject）
 *
 *参考jQuery.Deferred
 *url: http://api.jquery.com/category/deferred-object/
 */
Es6_Promise.deferred = function() { // 延迟对象
    let defer = {};
    defer.Es6_Promise = new Es6_Promise((resolve, reject) => {
        defer.resolve = resolve;
        defer.reject = reject;
    });
    return defer;
}

/**
 * Es6_Promise/A+规范测试
 * npm i -g Es6_Promises-aplus-tests
 * Es6_Promises-aplus-tests Es6_Promise.js
 */

try {
  module.exports = Es6_Promise
} catch (e) {
}



// let testa = new Es6_Promise(rs=>setTimeout(_=>rs(123),100));


// setTimeout(()=>testa.then(e=>console.log(e)),3000)


```