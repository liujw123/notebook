# lazyMan

``` js

    // 实现一个LazyMan，可以按照以下方式调用:
    LazyMan('Hank')
    // 输出:
    // Hi! This is Hank!

    LazyMan('Hank').sleep(10).eat('dinner')
    // 输出
    // Hi! This is Hank!
    // 等待10秒..
    // Wake up after 10
    // Eat dinner~

    LazyMan('Hank').eat('dinner').eat('supper')
    // 输出:
    // Hi This is Hank!
    // Eat dinner~ 
    // Eat supper~

    LazyMan('Hank').sleepFirst(5).eat('supper')
    // 输出:
    // 等待5秒
    // Wake up after 5
    // Hi This is Hank!
    // Eat supper
    // 以此类推。

```


#### 知识点

> **闭包，事件轮询机制，链式调用，队列**


## 大致思路

1. 存储需要队列信息
2. 同步执行方法，放进待执行队列，每个function设置回调
3. 异步调动队列第一个方法
4. 获取队列，拿出一个执行


### 实现一 (ES6)
```js
    class _LazyMan {
        constructor(name) {
            this.tasks = [];
            const task = () => {
                console.log(`Hi! This is ${name}`);
                this.next();
            }
            this.tasks.push(task);
            setTimeout(() => {               // 把 this.next() 放到调用栈清空之后执行
                this.next();
            }, 0);
        }
    
        next() {
            const task = this.tasks.shift(); // 取第一个任务执行
            task && task();
        }
    
        sleep(time) {
            this._sleepWrapper(time, false);
            return this;                     // 链式调用
        }
    
        sleepFirst(time) {
            this._sleepWrapper(time, true);
            return this;
        }
    
        _sleepWrapper(time, first) {
            const task = () => {
                setTimeout(() => {
                console.log(`Wake up after ${time}`);
                this.next();
                }, time * 1000)
            }
            if (first) {
                this.tasks.unshift(task);     // 放到任务队列顶部
            } else {
                this.tasks.push(task);        // 放到任务队列尾部
            }
        }
    
        eat(name) {
            const task = () => {
                console.log(`Eat ${name}`);
                this.next();
            }
            this.tasks.push(task);
            return this;
        }
    }

    function LazyMan(name) {
        return new _LazyMan(name);
    }
```
---

### 实现一 (ES5)

```js 
    // 构造函数
    function _lazyMan (name){
        this.taskList = [];  // 储存任务
        var task = function(){
            console.log('this is '+name);
            this.next();
        }
        this.taskList.push(task);

        this.next = next.bind(this);

        setTimeout(()=>this.next(),0)
    };

    // 抽出一个任务执行
    function next(){
        if(this.taskList.length<1)return
        var task = this.taskList.shift();
        task&&task.call(this);
    }

    _lazyMan.prototype.eat = function(any){
        var task = function(){
            console.log('Eat  '+ any +'~');
            this.next();
        }
        this.taskList.push(task);
        return this;
    }

    _lazyMan.prototype.sleep = function(time){
        var task = function(){
            setTimeout(function(){
                console.log('Wake up after ' + time + '~');
                this.next();
            }.bind(this),time*1000)
        }
        this.taskList.push(task);
        return this;
    }

    _lazyMan.prototype.sleepFirst = function(time){
        var task = function(){
            setTimeout(function(){
                console.log('Wake up after ' + time + '~');
                this.next();
            }.bind(this),time*1000)
        }
        this.taskList.unshift(task);
        return this;
    }

    var LazyMan = function(){return new _lazyMan};
```

---

### 实现二 (ES5)

```js
    (function(global, undefined){
        var taskList = [];
        // 类
        function LazyMan(){};
        LazyMan.prototype.eat = function(str){
            subscribe("eat", str);
            return this;
        };
        LazyMan.prototype.sleep = function(num){
            subscribe("sleep", num);
            return this;
        };
        LazyMan.prototype.sleepFirst = function(num){
            subscribe("sleepFirst", num);
            return this;
        };
        
        // 订阅
        function subscribe(){
            var param = {},
                args = Array.prototype.slice.call(arguments);
            if(args.length < 1){
                throw new Error("subscribe 参数不能为空!");
            }
            param.msg = args[0];
            param.args = args.slice(1); // 函数的参数列表
            if(param.msg == "sleepFirst"){
                taskList.unshift(param);
            }else{
                taskList.push(param);
            }
        }
        // 发布
        function publish(){
            if(taskList.length > 0){
                run(taskList.shift());
            }
        }
        // 判断执行
        function run(option){
            var msg = option.msg,
                args = option.args;
            switch(msg){
                case "lazyMan": lazyMan.apply(null, args);break;
                case "eat": eat.apply(null, args);break;
                case "sleep": sleep.apply(null,args);break;
                case "sleepFirst": sleepFirst.apply(null,args);break;
                default:;
            }
        }
        // 具体方法
        function lazyMan(str){
            lazyManLog("Hi!This is "+ str +"!");
            publish();
        }
        function eat(str){
            lazyManLog("Eat "+ str +"~");
            publish();
        }
        function sleep(num){
            setTimeout(function(){
                lazyManLog("Wake up after "+ num);
                publish();
            }, num*1000);
            
        }
        function sleepFirst(num){
            setTimeout(function(){
                lazyManLog("Wake up after "+ num);
                publish();
            }, num*1000);
        }
        // 输出文字
        function lazyManLog(str){
            console.log(str);
        }
        // 暴露接口
        global.LazyMan = function(str){
            subscribe("lazyMan", str);
            setTimeout(function(){
                console.warn(JSON.stringify(taskList))
                publish();
            }, 0);
            return new LazyMan();
        };
    })(global);
```

--- 