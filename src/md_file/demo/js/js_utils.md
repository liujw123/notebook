# js 工具方法

>整合js方法，工具类

---

#### 检测是否为空对象

```js
    function isEmptyObj(obj){
        for(let i in obj){
            return false
        }
        return true
    }
```

---

#### 同步限制点击

```js
    // preventTime 限制多久才能再次
    let preventclick = (()=>{
        var oldtime = '';
        return function preventclick(preventTime){
            if(oldtime==''){
                oldtime = new Date().getTime();
                return true;
            }else{
                var newtime = new Date().getTime();
                if(newtime - oldtime > preventTime){
                    oldtime = new Date().getTime();
                    return true;
                }else{
                    return false;
                }
            }
        }
    })()
```

---

#### 判断平台系统

```js
    // 判断 android/ios
    let isSystem=(...agrs)=>{
        let u = navigator.userAgent;
        let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //android
        let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);               //ios终端
        let currentSys = isAndroid?'android':isIOS?'ios':'unKonwSys'
        return agrs.some(e=>e==currentSys);
    }
```

---

#### 多维数组平铺

```js
    let steamroller = function(arr){
        while(arr.some(item=>Array.isArray(item))){arr=[].concat(...arr)};
        return arr
    }
```

---

#### 防抖

```js
    
    function debounce(func, wait, immediate) {
        let timeout, args, context, timestamp, result;
        const later = function() {
            // 据上一次触发时间间隔
            const last = +new Date() - timestamp;
            // 上次被包装函数被调用时间间隔last小于设定时间间隔wait
            if (last < wait && last > 0) {
            timeout = setTimeout(later, wait - last);
            } else {
            timeout = null;
            // 如果设定为immediate===true，因为开始边界已经调用过了此处无需调用
            if (!immediate) {
                result = func.apply(context, args);
                if (!timeout) context = args = null;
            }
            }
        }
        return function(...args) {
            context = this;
            timestamp = +new Date();
            const callNow = immediate && !timeout;
            // 如果延时不存在，重新设定延时
            if (!timeout) timeout = setTimeout(later, wait);
            if (callNow) {
            result = func.apply(context, args);
            context = args = null;
            }
            return result;
        }
    }

```

---

#### 深克隆

```js
    
    function jsonStr(data){
        return encodeURIComponent(JSON.stringify(data))
    }
    function jsonPar(json){
        return JSON.parse(decodeURIComponent(json))
    }

```

---


#### 正则验证

```js
    
    // 手机号码
    const phoneReg = new RegExp(/^1(3|4|5|6|7|8|9)\d{9}$/);

```

---