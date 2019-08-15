# 小程序腾讯云IM-实现简单C2C单聊---登陆（独立模式）

>详细说明，移步[官方文档](https://cloud.tencent.com/document/product/269/1595)      
>[im调试工具](https://avc.cloud.tencent.com/im/APITester/APITester.html)   

1. 到[腾讯云通信（IM）控制台](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent.com%2Favc)创建和生成密匙等信息，
2. 引入IM SDK [webim_wx.js](https://cloud.tencent.com/document/product/269/33144)
3. 请求后台生成登陆sign
4. 调用 SDK 登陆
5. 登陆成功后就可以调用 SDK 功能

**identifierNick 的值只在初始化的登录时有效（第一次登录某 identifier)，初始化帐号后的昵称修改，需要调用 (setProfilePortrait)[https://cloud.tencent.com/document/product/269/1599] 接口。**
```js
    // im_handler.js
    
   /*
    * 登录 im
    */
    let im = require('../../utils/webim_wx.min.js');

    /**
    * 登录 im
    */
    /*
    * 使用闭包，实现单独作用域，保留对象的引用，新消息监听回调绑定
    */
    
    let login = (function(){
        // 绑定 onMsgNotify 不同页面动态设置函数
        let MsgNotifyObj = {};
        return function({loginInfo, MsgNotifyCallBack}) {
            MsgNotifyObj.MsgNotifyCallBack = MsgNotifyCallBack;
            // 检查是否登录返回 true 和 false,不登录则重新登录
            if(im.checkLogin())return Promise.resolve('正在登陆中');

            im.Log.warn('开始登录 im')
            if (!loginInfo.imId || !loginInfo.userSig){
                im.Log.error("登录 im 失败[im 数据未初始化完毕]");
                return Promise.reject("登录 im 失败[im 数据未初始化完毕]")
            }
             
            // 获取当前用户身份
            var _loginInfo = {
                'sdkAppID': loginInfo.sdkAppID,             // 用户标识接入 SDK 的应用 ID，必填
                'appIDAt3rd': loginInfo.sdkAppID,           // App 用户使用 OAuth 授权体系分配的 Appid，必填
                'accountType': loginInfo.accountType,       // 帐号体系集成中的 accountType，必填
                'identifier': loginInfo.imId,               // 当前用户帐号，必填
                'identifierNick': loginInfo.imName,         // 当前用户昵称，选填
                'userSig': loginInfo.userSig,               // 鉴权 Token，必填
            }
            // 指定监听事件
            var listeners = {
                "onConnNotify": onConnNotify,                   // 监听连接状态回调变化事件,必填
                "onMsgNotify": onMsgNotify.bind(MsgNotifyObj)   // 监听新消息回调变化事件,必填，绑定MsgNotifyObj指针，实现登录后动态修改cb函数，从而在不同页面实现回调
            }
            //其他对象，选填
            var options = {
                'isAccessFormalEnv': true,  // 是否访问正式环境，默认访问正式，选填
                'isLogOn': false            // 是否开启控制台打印日志,默认开启，选填
            }
        
            // sdk 登录（独立模式）
            return new Promise((rs,rj)=>{
                im.login(_loginInfo, listeners, options, 
                    function (resp) {
                        im.Log.warn('登录 im 成功')
                        rs('重新登陆成功');
                    }, 
                    function (err) {
                        rj(err)
                        im.Log.error(err)
                    }
                )
            })
        }
    })()


    function onMsgNotify(newMsgList) {
        // 当有新消息时触发page函数 ，，，调用login，fnName
        this.MsgNotifyCallBack&&this.MsgNotifyCallBack(newMsgList);
        // 如果有新消息，并且处在会话列表界面上
        // if (newMsgList && contactListThat) {
        //   contactListThat.initRecentContactList()
        // }
    }

    /**
    * 监听连接状态回调变化事件
    */
    function onConnNotify(resp) {
        switch (resp.ErrorCode) {
            case im.CONNECTION_STATUS.ON:
                im.Log.warn('连接状态正常...')
            break
            case im.CONNECTION_STATUS.OFF:
                im.Log.warn('连接已断开，无法收到新消息，请检查下你的网络是否正常')
            break
            default:
                im.Log.error('未知连接状态,status=' + resp.ErrorCode)
            break
        }
    }
```





