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
