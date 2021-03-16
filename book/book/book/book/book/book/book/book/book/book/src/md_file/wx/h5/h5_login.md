## 微信H5 授权

**vue hash 模式**    

>[官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140842)


**同意授权后，授权不再弹窗，直接获取code**

1. 设置分享地址参数。
2. 切换分享地址-->授权成功-->携带code重定向所设置链接-->获取地址栏code。


```js
    // 跳转微信授权页
    /*
     * 
     * appid => 
     * redirect_url => 授权成功后重定向地址
     * scope => 应用授权作用域
     * response_type => 返回类型
     * state => 重定向后会带上state参数,用于传递参数（由于是地址栏传参授权，state参数注意长度限制）
     *
     */
    function getCode({appid,redirect_url=encodeURIComponent(location.href),scope='snsapi_userinfo',response_type='code',state='NO_STATE'}) {
        if(getQueryString('code')!=null)return;   								    // 判断当前地址是否存在code  存在则停止，调用方法，获取code传后台获取用户信息                                       // 授权成功后重定向的地址（重定向后地址会携带code） / getQueryString方法可获取 
        let author_url = 'https://open.weixin.qq.com/connect/oauth2/authorize';     // 微信授权地址
        let wx_get_author_url = `${author_url}?appid=${appid}&redirect_uri=${redirect_url}&response_type=${response_type}&scope=${scope}&state=${state}#wechat_redirect`;
        window.location.href=wx_get_author_url;                                     // 切换当前地址 跳转微信授权页授权
    }
    
    
    // 获取code 登陆流程
    function loginServer(){
        if(getQueryString('code')==null)return;
        let wx_code = getQueryString('code');
        /*
         *
         *...
         *...
         *...走正常登陆流程
         *
         */
    }

    // 获取 地址参数
    function getQueryString(name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
        var r = window.location.search.substr(1).match(reg);
        if (r != null) return unescape(r[2]);
        return null;
    }

```

### 注：

1. 授权是跳转微信页面发起授权弹窗，授权成功后重定向，对单页不友好，整个页面重载刷新，记录比较多信息只能用缓存比较好， （state可带参数，但设置和获取不便，貌似浏览器对域名长度有限制）。
2. code 会存在过期，后台获取jsapi_ticket（有效期7200秒，开发者必须在自己的服务全局缓存jsapi_ticket）。