# 任务队列

* 同步与异步、宏任务和微任务分别是函数两个不同维度的描述。
* 异步任务：setTimeout和setInterval、ajax、事件绑定等
* 同步任务：除了异步任务外的所有任务
* 微任务：process.nextTick和 Promise后的theny语句和catch语句等
* 宏任务：除了微任务以外的所有任务 

### 执行顺序   
> 先同步再异步，在此基础上先宏任务再微任务   

```js 

    setTimeout(function () {
        new Promise(function (resolve, reject) {
            console.log(1);
            resolve();
        }).then(function () {
            console.log(2)
        })
        console.log(3);
    }, 0)

    async1();

    new Promise(function (resolve, reject) {
        console.log(4);
        setTimeout(resolve,0)
        // resolve();
    }).then(function () {
        console.log(5)
    })

    console.log(6)

    async function async1(){
        console.log(7);
        await console.log(8);
        console.log(9)
    }
    // 784613592

```