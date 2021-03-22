# js utils

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

#### [多维数组平铺](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/flat)

```js
    let steamroller = arr =>{
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


#### 动态修改htmlfontsize值

```js
    
    (function (win,doc) {
        function setSize() {
            doc.documentElement.style.fontSize=document.documentElement.clientWidth+'px';
        }
        setSize();
        win.addEventListener('resize',setSize,false)
    })(window,document)


    // scss 计算函数
    @function r($px){
        @return (1rem/750px)*$px;
    }
```

---


#### 处理iOS 微信客户端6.7.4 键盘收起页面未下移bug


```js
    (/iphone|ipod|ipad/i.test(navigator.appVersion)) && document.addEventListener('blur', e=> {
        // 这里加了个类型判断，因为a等元素也会触发blur事件
        ['input', 'textarea'].includes(e.target.localName) && document.body.scrollIntoView(false)
    }, true)
```


---


#### 判断是否同一天   

```js

    export function  isSameDay(timeStampA, timeStampB) {
        let dateA = new Date(timeStampA);
        let dateB = new Date(timeStampB);
        return (dateA.setHours(0, 0, 0, 0) == dateB.setHours(0, 0, 0, 0));
    }

```

---


#### 获取范围内随机数   

```js

    function getRandom(min, max) {
        return Math.round(Math.random() * (max - min)) + min;
    }

```

---


#### 获取范围内随机数   

```js

    function getRandom(min, max) {
        return Math.round(Math.random() * (max - min)) + min;
    }

```




#### 格式化日期

```js
    
    export const formatTime = date => {
        const year = date.getFullYear()
        const month = date.getMonth() + 1
        const day = date.getDate()
        const hour = date.getHours()
        const minute = date.getMinutes()
        const second = date.getSeconds()

        return [year, month, day].map(formatNumber).join('-') + ' ' + [hour, minute, second].map(formatNumber).join(':')
    }

    export const formatDate = ({ date= new Date(), partition= '-' }) => {
        let _date;
        if(typeof (date) === 'string'&&date.indexOf('-')!=-1){
            _date = new Date(date.replace(/\-/g,'/'))
        }else{
            _date = new Date(date)
        }
        const year = _date.getFullYear();
        const month = _date.getMonth() + 1;
        const day = _date.getDate();
        
        if(partition == 'zh')return `${year}年${month}月${day}日`
        return [year, month, day].map(formatNumber).join(partition);
    }

    export const formatNumber = n => {
        n = n.toString()
        return n[1] ? n : '0' + n
    }


```
