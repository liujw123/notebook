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