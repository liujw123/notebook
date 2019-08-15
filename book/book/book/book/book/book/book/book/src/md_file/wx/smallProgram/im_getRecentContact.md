# 小程序腾讯云IM-实现简单C2C单聊---获取最近联系人（独立模式）   
>详细说明，移步[官方文档](https://cloud.tencent.com/document/product/269/1595)        
>[im调试工具](https://avc.cloud.tencent.com/im/APITester/APITester.html)      

1. [登陆](./im_login.md);
2. 调用sdk获取最近联系人列表数据
3. 初始化消息，获取未读消息
4. 处理数据格式
5. 赋值data，显示到界面


```js
    let imhandler = require('../../utils/im_handler.js'); // 这个是所有 im 事件的 js
    import Promise from '../../utils/promise.js'; // 目前小程序原生取消支持Promise
    import regeneratorRuntime, { async } from '../../utils/regenerator-runtime.js'  //async /小程序原生不支持async
    // 获取应用实例
    const app = getApp()

    onShow(){
        app.initImParams(()=>{ // 获取签名,初始化登陆所需的数据数据
            imhandler.login({   // 调用im登陆
                loginInfo:app.data.im,
                MsgNotifyCallBack:this.formatContactList,  // 有新消息时，触发的回调
            })
            .then(this.formatContactList, // 加载数据，处理数据
            err=>{
                console.warn('im 登陆失败-->',err)
            })
            .catch(err=>{
                console.warn('加载最近联系人失败--->',err)
                util.showNone('加载最近联系人失败');
            })
        }),
    },
    // 处理最近联系人列表信息、格式化
    async formatContactList(){
        try{
            let contactArr = await Promise.all([this.getRecentContactList(),this.getConversationList()]);
            let contactList = contactArr[0].map(e=>{
                for(let i in contactArr[1]){
                    var session = contactArr[1][i]
                    if (session.unread() > 0&&e.friendId === session.id())e.unreadMsgCount = session.unread();
                }
                return e
            });


            // 设置联系人列表
            this.setData({
                contactList: contactList
            })
            // contactList 最终所需要的最近联系人列表数据
        }catch(err){
            console.warn('获取最近联系人列表、未读消息失败--->',err);
        }
    },

    
    // 获取最近联系人列表
    getRecentContactList(){
        return new Promise((rs,rj)=>{
            im.getRecentContactList({ 'Count': 100 }, // 获取联系人数量，上限100
            res=>{
                if(res.SessionItem && res.SessionItem.length > 0){
                    rs(res.SessionItem.map(e=>{
                        return{
                            'friendId': e.To_Account,
                            'friendName': e.C2cNick,
                            'friendAvatarUrl': e.C2cImage,
                            'msgTime': util.getDateDiff(e.MsgTimeStamp * 1000), // 格式化时间
                            'msg': e.MsgShow,
                            'unreadMsgCount': e.UnreadMsgCount
                        }
                    }));
                }else{
                    rs([]);
                }
                console.log('获取最近联系人列表成功--->',res);
            },
            err=>{
                console.warn('获取最近联系人列表失败--->',err)
                rj(err);
            })
        })
    },

    
    // 获取会话对象列表
    getConversationList(){
        // 初始化消息
        return new Promise((rs,rj)=>{
            im.syncMsgs(
                res=>{
                    console.warn('初始化会话信息成功--->',res);
                    rs(im.MsgStore.sessMap()) // 获取未读消息对象
                },
                err=>{
                    console.warn('初始化会话信息失败--->',err);
                    rj(err);
                }
            )
        })
    },

```

