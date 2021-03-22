# 函数防抖(debounce)   

*在事件被触发n秒后再执行回调，如果在这n秒内又被触发，则重新计时。*   


```js

    let debounce = (function(){
        let timer;
        return (function(fn,ms){
            clearTimeout(timer);
            timer = setTimeout(fn, ms);
        })
    })()

```

# 函数节流(throttle)

*规定一个单位时间，在这个单位时间内，只能有一次触发事件的回调函数执行，如果在同一个单位时间内某事件被触发多次，只有一次能生效。*   


```js

    let throttle = (function(){
        let lastTime = null;
        return (function(fn,ms){
            if(!!lastTime&&(+new Date()-lastTime)<=ms){
                return;
            }
            lastTime = + new Date();
            fn();
        })
    })();

```