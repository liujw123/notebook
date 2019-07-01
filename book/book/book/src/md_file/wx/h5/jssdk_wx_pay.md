## 微信h5 jssdk 调起微信支付

**vue hash 模式**  

> 只做简单介绍，具体注意事项查看[官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)



1. 登陆商户号对应的商户平台--"产品中心"--"开发配置"配置 支付路径。
2. 在需要支付的页面[配置注入 js-sdk](./jssdk_config.md)。
3. 调用 js sdk 微信支付 api。

```js
SDK_config([
    'chooseWXPay'
])

 // 调用后台接口 生成 签名等信息后调用 sdk api
let wx_sdk_pay = data =>{
    return new Promise((rs,rj)=>{
        wx.ready(function(e){
            wx.chooseWXPay({
                'timestamp': data.timestamp,            // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
                'nonceStr': data.noncestr,              // 支付签名随机串，不长于 32 位
                'package': data.package,                // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
                'signType': data.signType,              // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
                'paySign': data.sign,                   // 支付签名
                success: function (res) {
                    // 支付成功后的回调函数
                    // console.log('chooseWXPay_suc:',res)
                    res['pay_status'] = 'success';
                    rs(res);
                },
                cancel:function(res){
                    res['pay_status'] = 'cancel';
                    rs(res);
                },
                fail:err=>{
                    console.warn('调用支付sdk失败 chooseWXPay_err',JSON.stringify(err))
                    rj(err)
                }
            });
        });
    })
}

```
---

#### **安卓对会对当前页地址进行验证，而ios则会对首次进入或最后一次刷新地址进行验证**   


```js

//当前url更改/ sdk 支付 验证地址失败
let compatible_wx_pay = (curHref = location.href,isReturn=false)=>{
    let u = navigator.userAgent;
    if(curHref.indexOf('?a=1')==-1){                    //判断是否 存在'?a=1'忽略后面参数/识别为当前目录生成订单（兼容安卓）
    	// if(u.indexOf('Android') > -1 || u.indexOf('Linux') > -1){
            let curHrefArr = curHref.split('#');
            if(isReturn)return `${curHrefArr[0]}?a=1#${curHrefArr[1]}`;
    		location.href = `${curHrefArr[0]}?a=1#${curHrefArr[1]}`
    	// }
    }
    if(isReturn)return curHref;
}

```

---

###注意   

1. 商户平台配置支付路径上限5个。   
2. 当网站只通过单一入口进入网站，商户平台ios支付路径只需配置入口网址（因为ios端进入网站后路由切换不会显式地在域名上更改，复制域名始终一样）而安卓则需要配置每个页面的支付路径（注：包含hash部分与路由路径，无需具体到文件），**但是如果带hash和路径进入网站，ios支付识别支付路径则会检验首次进入页面地址包含hash部分与路由路径或者网站整体刷新的最后一次地址**，由于商户只支持5个域名会出现配置上限；

> uni-vue-hash 多个页面支付解决方案   
    1. 对hash部分进行忽略，在hash前加`?`(即`?#/...`)或者`?a=1#/`,这样支付验证就会忽略hash部分，从而商户平台只需配置不带hash部分的支付路径就可以。注意**安卓即使忽略hash，路由切换也会改变被忽略hash路径和参数，而ios则首次进入会从地址获取hash后路径和参数，但是后续路由切换则不会修改被忽略后的hash部分，切换页面后被忽略hash部分还是会和刚进来时一样。** 

    2. 统一支付页面，用浏览器路由location切换到支付页，这样只需要配置支付页的支付路径就可以。
