## 微信h5 jssdk 配置 (vue 单页 hash模式)

> 只做简单介绍，具体注意事项查看官方文档   

> [微信JS-SDK说明文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)

1. **先到公众号的基本开发配置，设置js接口安全域名，不然出现配置失败**   
2. 安装weixin-js-sdk 
    * 命令 `npm install weixin-js-sdk`
3. 引入配置

---

### 配置代码

```

    import wx from './weixin-js-sdk';

    let SDK_config = jsApiList => {
        return new Promise((rs,rj)=>{
            _getConfigSign(_get_url_compatible_ios_config())    // 后台获取签名等其他信息/
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
                    // console.log('wx.ready')
                    console.log('ready:',JSON.stringify(e));
                    rs(e);
                });
                wx.error(function(err){
                    // console.warn('wx.error');
                    console.warn('js-sdk配置失败-->',JSON.stringify(err));
                    rj('配置失败：'+JSON.stringify(err))
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

    //url处理/兼容ios 注入sdk 出现签名失败情况
    function _get_url_compatible_ios_config(){
        //兼容ios url 签名失败  start
        let url = ''
        let u = navigator.userAgent;
        let isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //android
        let isIOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/);               //ios终端
        if (isAndroid) {
            url=location.href
        }
        if (isIOS) {
            url=location.href.split('#')[0]  //hash后面的部分如果带上ios中config会不对
        }
        return url
        //兼容ios url 签名失败  
    }
    
```
--- 
### 坑点

1. url处理/兼容ios
2. 每次url变化之后都需要重新微信jssdk授权，要用到的页面，mounted 注册
3. 设置分享，链接中存在中文，进行encodeURIComponent()，安卓手机会自动encodeURIComponent，ios不会。 


