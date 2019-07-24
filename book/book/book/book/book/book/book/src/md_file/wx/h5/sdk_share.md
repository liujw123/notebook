## 微信h5 sdk 设置点击菜单分享 (朋友、朋友圈、QQ、等)   

**vue hash 模式**   

> 只做简单介绍，具体注意事项查看[官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)

1. 在需要设置的页面[配置注入 js-sdk](./jssdk_config.md)
2. 在需要设置的页面，调用分享设置方法。
3. 设置是否成功看心情。。。
---
```js   

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
            type:'link',            // 分享类型,music、video或link，不填默认为link  (貌似可以兼容部分华为分享参数携带失败///单分享链接，配置不影响)
            dataUrl:shareLink,      // 如果type是music或video，则要提供数据链接，默认为空 (貌似可以兼容部分华为分享参数携带失败///单分享链接，配置不影响)
            success: function () {
                console.warn('分享设置成功，设置连接为-->'+shareLink);  
            },
            fail:err=>{
                console.warn('分享设置失败-->：',JSON.stringify(err))
            }
        }

        // 文档说将废弃 ， 没有废弃。。。 updateAppMessageShareData/updateTimelineShareData 单独配置这两个，分享设置大部分手机不成功。。。
        // 全部api堆上就对了。。。

        wx.updateAppMessageShareData(shareObj);     //   自定义“分享给朋友”及“分享到QQ”按钮的分享内容（1.4.0）
        wx.updateTimelineShareData(shareObj);       //   自定义“分享到朋友圈”及“分享到QQ空间”按钮的分享内容（1.4.0）
        wx.onMenuShareTimeline(shareObj);           //   获取“分享到朋友圈”按钮点击状态及自定义分享内容接口（即将废弃）
        wx.onMenuShareQQ(shareObj);                 //   获取“分享到QQ”按钮点击状态及自定义分享内容接口（即将废弃）
        wx.onMenuShareWeibo(shareObj);              //   获取“分享到腾讯微博”按钮点击状态及自定义分享内容接口
        wx.onMenuShareQZone(shareObj);              //   获取“分享到QQ空间”按钮点击状态及自定义分享内容接口（即将废弃）
    })
}

```

---

### 坑
1. **设置分享，链接中存在中文，进行encodeURIComponent()，安卓手机会自动encodeURIComponent，ios不会。**
2. **不知道是否 vue hash 模式问题，分享设置链接会出现设置失败，标题等其他信息设置成功，只有链接失败。（华为出现几率大），，，未找到解决方案**
3. **若全局配置统一分享可在app.vue注入设置。（个别差异分享，要在页面卸载时，重新设置全局分享参数）**