# 小程序腾讯云IM-实现简单C2C单聊---获取签名（独立模式）   
> 详细说明，移步[官方文档](https://cloud.tencent.com/document/product/269/1595)        
> [im调试工具](https://avc.cloud.tencent.com/im/APITester/APITester.html)      

1. 获取用户信息，进行签名生成


```js
    //app.js

    let util = require('./utils/util.js');  // 转换时间插件
    let {wxApiPromise:wxp} = util;          // 微信api Promise 封装
    import Promise from './utils/promise.js';
    import regeneratorRuntime, { async } from './utils/regenerator-runtime.js'  //async
    App({
        data: {
            im: {
            sdkAppID: 1400120449,   // 用户标识接入 SDK 的应用 ID，必填 
            accountType: 36832,     // 帐号体系集成中的 accountType，必填
            accountMode: 0,         // 帐号模式，0 - 独立模式 1 - 托管模式
            imId: null,             // 用户的 id
            imName: null,           // 用户的 im 名称
            imAvatarUrl: null,      // 用户的 im 头像 url
            userSig: null           // 用户通过 imId 向后台申请的签名值 sig
            }
        },
        /**
        * 初始化 im 参数，返回成功回调
        * 模拟请求签名
        */
        initImParams: async function (){
            try{
                // 可以获取用户信息 可以调用getUserInfo接口
                this.data.im.imName = 'user_liu'; // 名字 
                this.data.im.imAvatarUrl = '.jpg'; // 头像
            
                let appid = 'wx324ec51',
                    secret = '706d2488e61959',
                    generatedSigUrl = 'http://www.ex.com';

                if(!!this.data.im.imId&&!!this.data.im.userSig)return this.data.im;
            
                // 通过code appid secret 换取 openID 实际开发放后台获取
                let loginRes = await wxp.login();
                let uri = '?appid=' + appid + '&secret=' + secret + '&js_code=' + loginRes.code + '&grant_type=authorization_code';
                let url = 'https://api.weixin.qq.com/sns/jscode2session' + uri;
                let sessionRes = await wxp.request({url:url,method: 'GET'});

                // 通过 openID 获取【腾讯 im】签名值
                let header = { "Content-Type": "application/x-www-form-urlencoded" };
                let data = { "identifier": sessionRes.data.openid};
                let signRes = await wxp.request({url:generatedSigUrl,header,method: 'POST',data});
                // console.warn(signRes.data)
                this.data.im.imId = sessionRes.data.openid;  // 用作登陆账号
                this.data.im.userSig = signRes.data;

                // return {
                //   sign:signRes.data, 
                //   openid:sessionRes.data.openid //openid 用作登陆账号
                // }
                return this.data.im;
            }catch(err){
                console.warn('获取签名失败---->',err);
            }
        }
    })

```

