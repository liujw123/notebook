## 微信h5 jssdk 配置 

**vue hash 模式**  

> 只做简单介绍，具体注意事项查看[官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)


1. **先到公众号的基本开发配置，设置js接口安全域名（单域名就好，不带协议，不带路径）**   
2. 安装 weixin-js-sdk 包
    * 命令 `npm install weixin-js-sdk`
3. 引入包、配置注入sdk
4. 注入成功，就可以愉快地踩坑了

---

### 配置代码

```js

    import wx from './weixin-js-sdk';

    let SDK_config = jsApiList => {
        return new Promise((rs,rj)=>{
            _getConfigSign(_get_url_compatible_ios_config())    // 后台获取签名等其他配置信息/
            .then(res=>{
                wx.config({
                    debug:SDK_DEBUG,                            // 开启调试模式,调用的所有api的返回值会在客户端alert出来 bol
                    appId:res.appId,                            // 必填，公众号的唯一标识   str
                    timestamp:res.timestamp,                    // 必填，生成签名的时间戳 str
                    nonceStr:res.nonceStr,                      // 必填，生成签名的随机串 str
                    signature:res.signature,                    // 必填，签名   str
                    jsApiList:[...jsApiList],                   // 必填，需要使用的JS接口列表 arr
                })
                wx.ready(function(e){
                    console.log('ready:',JSON.stringify(e));
                    rs(e);

                    // config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。
                });
                wx.error(function(err){
                    console.warn('js-sdk配置失败-->',JSON.stringify(err));
                    rj(err);

                    // config信息验证失败会执行error函数，如签名过期导致验证失败，具体错误信息可以打开config的debug模式查看，也可以在返回的res参数中查看，对于SPA可以在这里更新签名。
                }); 
            })
        })
    }

    //后台请求获取js-sdk 配置信息 //签名/时间等
    function _getConfigSign(url){   // 获取当前地址进行生成签名
        return ServerData.sdk_config({url})
        .then(e=>{
            if(e.code == 0){
                return e.data
            }else{
                console.warn('获取签名失败'+JSON.stringify(e))
                return Promise.reject(e);
            }
        })
    }

    //url处理/兼容ios 注入 sdk 出现签名失败
    function _get_url_compatible_ios_config(){
        let u = navigator.userAgent;
        let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //android
        let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);               //ios终端
        if (isAndroid)return location.href;
        if (isIOS)return location.href.split('#')[0];   //hash后面的部分如果带上ios中config会不对
        return location.href
    }
    
```

--- 
### 坑

1. url处理/兼容ios。
2. 每次url变化之后都需要重新微信jssdk授权，要用到的页面，mounted 调用config注册sdk。
3. 设置分享，链接中存在中文，进行encodeURIComponent()，安卓手机会自动encodeURIComponent，ios不会。 


