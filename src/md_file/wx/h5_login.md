## 微信H5登陆

>[官方文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140842)

### 跳转微信登陆链接，获取code

**同意授权后，授权不再弹窗，直接获取code**

```
    // 跳转微信授权页
    function getCode(appid,) {
        if(getQueryString('code')!=null)return;   								    // 判断当前地址是否存在code  存在则停止，调用方法，获取code传后台获取用户信息
        let local = window.location.href;                                           // 授权成功后重定向的地址（重定向后地址会携带code） / getQueryString方法可获取 
        let author_url = 'https://open.weixin.qq.com/connect/oauth2/authorize';     // 微信授权地址
        let redirect_url = encodeURIComponent(local);                               // 获取code成功后重定向url
        let scope = 'snsapi_userinfo';                                              // 应用授权作用域
        let response_type = 'code';                                                 // 返回类型
        let state = 'NO_STATE';                                                     // 重定向后会带上state参数
        let wx_get_author_url = `${author_url}?appid=${appid}&redirect_uri=${redirect_url}&response_type=${response_type}&scope=${scope}&state=${state}#wechat_redirect`;
        window.location.href=wx_get_author_url;                                     // 切换当前地址 跳转微信授权页授权
    }
    
    // 获取 地址参数
    function getQueryString(name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
        var r = window.location.search.substr(1).match(reg);
        if (r != null) return unescape(r[2]);
        return null;
    }

    // 获取code 登陆流程
    function loginServer(){
        if(getQueryString('code')==null)return;
        let wx_code = getQueryString('code');
        ...
        ...
        ...走正常登陆流程
    }

```