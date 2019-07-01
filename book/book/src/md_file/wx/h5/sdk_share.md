## 微信h5 sdk 设置分享

**vue hash 模式**   

> 具体注意事项查看[官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)

1. 在需要设置的页面[配置注入 js-sdk](./jssdk_config.md)
2. 在需要设置的页面，调用分享设置方法。
---

```js   

SDK_config([
    'updateAppMessageShareData',
    'updateTimelineShareData',
    'onMenuShareTimeline',          // 以下两个是如果不注入配置，则安卓设置分享失败
    'onMenuShareAppMessage'
])


// 设置点击菜单分享 方法封装 
let wx_on_menu_share = ({
    title = '',                                                                       // 设置分享标题 str
    desc = '',                                                                        // 设置分享描述 str
    imgUrl = '',                                                                      // 设置分享图标 str
    routePath = '',                                                                   // 设置分享路径(hash单页路由跳转路径) str 
    query = {},                                                                       // 设置分享参数 obj
    shareLink = location.protocol + '//' + location.host + location.pathname + '#/'   // 设置分享域名(hash带'#/') str
}) => {
    let shareQuery = '',i=1;
    for(let key in query){
        i == 1?
        shareQuery+= '?' + key +'='+ query[key]+'':
        shareQuery+= '&' + key +'='+ query[key]+'';
        i++;
    }
        
    return wx.ready(()=>{           // config 进到 ready 才能设置
        let shareObj = {
            title,                  // 分享标题
            desc,                   // 分享描述
            imgUrl,                 // 分享图标
            link:shareLink + routePath + shareQuery,         // 分享链接
            type:'link',            // 分享类型,music、video或link，不填默认为link  
            dataUrl:shareLink,      // 如果type是music或video，则要提供数据链接，默认为空 
            success: function () {
                console.warn('分享设置成功，设置连接为-->'+shareLink);  
            },
            fail:err=>{
                console.warn('分享设置失败-->：',JSON.stringify(err))
            }
        }

        // 文档说将废弃 ， 没有废弃。。。 updateAppMessageShareData/updateTimelineShareData 单独配置这两个，分享设置安卓大部分手机不成功。。。

        // ios 仅配置前两个成功设置
        wx.updateAppMessageShareData(shareObj);     //   自定义“分享给朋友”及“分享到QQ”按钮的分享内容（1.4.0）
        wx.updateTimelineShareData(shareObj);       //   自定义“分享到朋友圈”及“分享到QQ空间”按钮的分享内容（1.4.0）
        // 兼容安卓，需要把下面加上
        wx.onMenuShareTimeline(shareObj);           //   获取“分享到朋友圈”按钮点击状态及自定义分享内容接口（即将废弃）
        wx.onMenuShareAppMessage(shareObj);         //   获取“分享给朋友”按钮点击状态及自定义分享内容接口（即将废弃）
        // wx.onMenuShareQQ(shareObj);              //   获取“分享到QQ”按钮点击状态及自定义分享内容接口（即将废弃）
        // wx.onMenuShareWeibo(shareObj);              //   获取“分享到腾讯微博”按钮点击状态及自定义分享内容接口
        // wx.onMenuShareQZone(shareObj);              //   获取“分享到QQ空间”按钮点击状态及自定义分享内容接口（即将废弃）
    })
}

```

---

### 注：   

1. **设置分享，链接中存在中文，进行encodeURIComponent()，安卓手机会自动encodeURIComponent，ios不会。**
2. **单页spa，只要在某一页面设置分享，设置后，在没有重新设置的情况下，后续不重载的情况下，路由跳转所有页面点击分享都会应用上一次所设置的分享信息**
3. **hash模式，安卓在不带有hash路径(即不带`#/`)进入网站，会导致设置分享时，所设置的域名hash部分后被打断包含hash（`#/...`），参数与路径等信息无法进行分享**