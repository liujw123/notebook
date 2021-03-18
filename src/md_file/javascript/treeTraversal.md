# 多叉树遍历   




```js
    
     //递归实现
    console.time('------------- 递归实现 ------------------');
    var parseTreeJson = async function(treeNodes){
        if (!treeNodes || !treeNodes.length) return;

        for (var i = 0, len = treeNodes.length; i < len; i++) {

            var childs = treeNodes[i].children;
            
            console.log(treeNodes[i].id);

            if(childs && childs.length > 0){
                parseTreeJson(childs);
            }
        }
    };


    
    console.timeEnd('------------- 递归实现 ------------------');


    //非递归广度优先实现
    console.time('------------- 非递归广度优先实现 ------------------');
    
    var iterator1 = async function (treeNodes) {
        if (!treeNodes || !treeNodes.length) return;
        var stack = [];
        //先将第一层节点放入栈
        for (var i = 0, len = treeNodes.length; i < len; i++)stack.push(treeNodes[i]);
        var item;
        while (stack.length) {
            item = stack.shift();
            //  await sleep();
            console.log(item.id);

            //如果该节点有子节点，继续添加进入栈底
            if (item.children && item.children.length)stack = stack.concat(item.children);
        }
        };
        
    
    console.timeEnd('------------- 非递归广度优先实现 ------------------');

    //非递归深度优先实现
    console.time('------------- 非递归深度优先实现 ------------------');
    var iterator2 = function (treeNodes) {
        if (!treeNodes || !treeNodes.length) return;
        var stack = [];
        //先将第一层节点放入栈
        for (var i = 0, len = treeNodes.length; i < len; i++)stack.push(treeNodes[i]);
        var item;
        while (stack.length) {
            item = stack.shift();
            console.log(item.id);
            //如果该节点有子节点，继续添加进入栈顶
            if (item.children && item.children.length) {
                stack = item.children.concat(stack);
            }
        }
    };

    
    console.timeEnd('------------- 非递归深度优先实现 ------------------');


    var treeNodes = [
        {
            id: 1,
            name: '1',
            children: [
                {
                    id: 11,
                    name: '11',
                    children: [
                            {
                                id: 111,
                                name: '111',
                                children:[]
                            },
                            {
                                id: 112,
                                name: '112',
                                children:[
                                                    
                                ]
                            }
                    ]
                },
                {
                    id: 12,
                    name: '12',
                    children: []
                },
                {
                    id: 13,
                    name: '13',
                    children: [
                        {
                            id: 131,
                            name: '131',
                            children: [
                                {
                                    id: 1311,
                                    name: '1311',
                                    children: []
                            }
                            ]
                    }
                    ]
                }
            ],
            users: []
        },
        
    ];
    // parseTreeJson(treeNodes);
    // iterator1(treeNodes);
    // iterator2(treeNodes);
```