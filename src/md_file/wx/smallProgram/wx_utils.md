### 小程序常用工具类封装

#### 路由跳转

```js
    
    let routeTo = (url,type) => {
        switch(type){
            case 'nT':
            return wx.navigateTo({url});
            case 'rT':
            return wx.redirectTo({url});
            case 'rL':
            return wx.reLaunch({url});
            case 'sT':
            return wx.switchTab({url});
            default: wx.navigateBack({delta: 1})
            break
        }
    }

```

---

#### 提示弹窗

```js
    
    function showNone(txt){
        wx.hideToast()
        wx.showToast({
            mask:true,
            title: txt,
            icon: 'none',
            duration: 1500,
        })
    }

```

---

#### loading

```js
    
    function showLoad(title='加载中'){
        wx.showLoading({
            mask:true,
            title,
        })
    }

    function hideLoad(){
        wx.hideLoading()
    }

```

--- 

#### 获取节点信息

```js
    
    //then 异步
    //cb 貌似同步

    function getNodeMes(selector,cb){
        return new Promise(rs=>{
            wx.createSelectorQuery().select(selector)
            .boundingClientRect(res=>{
                cb&&cb(res)
                rs(res)
                // this.height = uni.getSystemInfoSync().windowHeight - res.height;
            }).exec()
        })
    }

```

---


#### 解析小程序码参数

```js
    
    function formatScene(scene = ''){
    let queryArr = scene.split('&');
    return queryArr.reduce((obj,e,i)=>{
        let _arr = e.split('=');
        obj[_arr[0]] = _arr[1];
        return obj;
    },{}) || {};
    }

```

---

#### base64 转图片

```js
    
    export const convertBase64 = function(base64data){
    const fsm = uni.getFileSystemManager();
    const FILE_BASE_NAME = 'tmp_base64src';//临时文件名
    return new Promise((resolve, reject) => {
        const [, format, bodyData] = /data:image\/(\w+);base64,(.*)/.exec(base64data) || []; //去掉 base64 的头信息：
        if (!format) {
            reject(new Error('ERROR_BASE64SRC_PARSE'));
        }
        const filePath = `${uni.env.USER_DATA_PATH}/${FILE_BASE_NAME}.${format}`; 
        const buffer = bodyData&&uni.base64ToArrayBuffer(bodyData);  //将 base64 数据转换为 ArrayBuffer 数据
        // console.log(uni.base64ToArrayBuffer(bodyData))
        fsm.writeFile({ //将 ArrayBuffer 写为本地用户路径的二进制图片文件 //图片文件路径在 uni.env.USER_DATA_PATH 中
            filePath,
            data: buffer,
            encoding: 'binary',
            success() {
                // console.log(buffer)
                resolve(filePath);
            },
            fail() {
                reject(new Error('ERROR_BASE64SRC_WRITE'));
            },
        });
    });
    }

```

---



#### 调用上页面方法

```js
    
    function previousPageFunction({fnName,query}){
        return new Promise((rs,rj)=>{
            try{
                if(getCurrentPages().length>1){
            console.log(getCurrentPages())
                    getCurrentPages()[getCurrentPages().length-2]['$vm'][fnName](query);
                    rs('success');
                }else{
                    console.error('当前路由栈为一，无法调取上一页数据');
                    rj('当前路由栈为一，无法调取上一页数据');
                }
            }catch(err){
                console.error('调用上一页面栈方法失败！',err);
                rj('调用上一页面栈方法失败！');
            }
        })
        
    }

```

---


#### api promise 化

```js
    
    export let promisify = api => {
        return (options, ...params) => {
            return new Promise((resolve, reject) => {
                api(Object.assign({}, options, { success: resolve, fail: reject }), ...params);
            });
        }
    }

```

---




