# 小程序腾讯云IM-实现简单C2C单聊---登陆（独立模式）

>详细说明，移步[官方文档](https://cloud.tencent.com/document/product/269/1595)      
>[im调试工具](https://avc.cloud.tencent.com/im/APITester/APITester.html)   

1. 到[腾讯云通信（IM）控制台](https://cloud.tencent.com/login?s_url=https%3A%2F%2Fconsole.cloud.tencent.com%2Favc)创建和生成密匙等信息
2. 引入IM SDK [webim_wx.js](https://cloud.tencent.com/document/product/269/33144)
3. 请求后台生成登陆sign
4. 调用 SDK 登陆
5. 登陆成功，调用 SDK 功能

```js
   /*
    * 登录 im
    */
    let im = require('../../utils/webim_wx.min.js');

    function im_login({loginInfo,MsgNotifyCallBackName,that}){

        // 检查登陆 im 登陆情况
        // 因为要登陆成功后才能对 SDK 其它功能调用，所以每次需要调用 SDK 其它方法都要检测登陆状态   
        // im 登陆成功会开启轮询

        if(im.checkLogin()){
            console.log('IM正在登陆');
            return Promise.resolve();
        }else{
            console.log('IM初始化登陆');
            return login(loginInfo);
        }
        // im 登陆
        function login(loginInfo) {
            im.Log.warn('开始登录 im');
            if (!loginInfo.imId || !loginInfo.userSig) {
                im.Log.error("登录 im 失败[im 数据未初始化完毕]")
                return Promise.reject('登录 im 失败[im 数据未初始化完毕]')
            }
            // 获取当前用户身份
            let loginInfo = {
                'sdkAppID': loginInfo.sdkAppID,           // 用户标识接入 SDK 的应用 ID，必填
                'appIDAt3rd': loginInfo.sdkAppID,         // App 用户使用 OAuth 授权体系分配的 Appid，必填
                'accountType': loginInfo.accountType,     // 帐号体系集成中的 accountType，必填
                'identifier': loginInfo.imId,             // 当前用户帐号，必填
                'identifierNick': loginInfo.imName,       // 当前用户昵称，选填
                'userSig': loginInfo.userSig,             // 鉴权 Token，必填 
            }

            // 指定监听事件
            let listeners = {
                "onConnNotify": onConnNotify,           // 监听连接状态回调变化事件,必填
                "onMsgNotify": onMsgNotify,             // 监听新消息回调变化事件,必填
                // "onKickedEventCall": onMsgNotify     // 被其他登录实例踢下线，选填
            }

            //其他对象，选填
            let options = {
                'isAccessFormalEnv': true,      // 是否访问正式环境，默认访问正式，选填
                'isLogOn': false                // 是否开启控制台打印日志,默认开启，选填
            }
            
            return new Promise((rs,rj)=>{
                // sdk 登录（独立模式）
                im.login(loginInfo, listeners, options, 
                function (resp) {
                    // console.log('登录 im 成功')
                    im.Log.warn('登录 im 成功');
                    rs(resp);
                }, 
                function (err) {
                    im.Log.error(err);
                    rj(err);
                })
            })
        }

        /**
        * 监听连接状态回调变化事件
        */
        function onConnNotify(resp) {
            switch (resp.ErrorCode) {
                case im.CONNECTION_STATUS.ON:
                    im.Log.warn('连接状态正常...');
                break
                case im.CONNECTION_STATUS.OFF:
                    im.Log.warn('连接已断开，无法收到新消息，请检查下你的网络是否正常')
                break
                default:
                    im.Log.error('未知连接状态,status=' + resp.ErrorCode)
                break
            }
        }

        /**
        * 监听新消息（初始化时会获取所有会话数组，随后只获取新会话数组）
        * newMsgList - 新消息数组
        */
        // 定义一个变量，识别不同页面，根据不同页面显示，调用不同方法
        function onMsgNotify(newMsgList) {
            that&&MsgNotifyCallBackName&&that[MsgNotifyCallBackName](newMsgList) // 触发页面方法
        }
    }
```





