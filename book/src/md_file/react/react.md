# react 文档笔记

### 组件生命周期

#### 组件的生命周期可分成三个状态：

|方法|说明|
|:-:|:-:|
|Mounting|已插入真实 DOM|
|Updating|正在被重新渲染|
|Unmounting|已移出真实 DOM|

#### 生命周期的方法有：

|方法|说明|
|:-:|:-:|
| componentWillMount | 在渲染前调用,在客户端也在服务端。 |
| componentDidMount  | 在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)。 |
| componentWillReceiveProps | 在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。 |
| shouldComponentUpdate | 返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。可以在你确认不需要更新组件时使用。|
| componentWillUpdate | 在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。 |
| componentDidUpdate  | 在组件完成更新后立即调用。在初始化时不会被调用。 |
| componentWillUnmount  | 在组件从 DOM 中移除之前立刻被调用。 |


### 正确地使用 State

##### 1. 不要直接修改 State    

>**构造函数是唯一可以给 this.state 赋值的地方**   

``` js
    // Wrong
    this.state.comment = 'Hello';

    // Correct
    this.setState({comment: 'Hello'});
    
```


##### 2. State 的更新可能是异步的   

+ 出于性能考虑，React 可能会把多个 setState() 调用合并成一个调用。   

+ 因为 this.props 和 this.state 可能会异步更新，所以你不要依赖他们的值来更新下一个状态。   

+ 例如，此代码可能会无法更新计数器：   
    ```js

        // Wrong
        this.setState({
            counter: this.state.counter + this.props.increment,
        });

        // 要解决这个问题，可以让 setState() 接收一个函数而不是一个对象。这个函数用上一个 state 作为第一个参数，将此次更新被应用时的 props 做为第二个参数：

        // Correct
        this.setState((state, props) => ({
            counter: state.counter + props.increment
        }));

        //上面使用了箭头函数，不过使用普通的函数也同样可以：

        // Correct
        this.setState(function(state, props) {
            return {
                counter: state.counter + props.increment
            };
        });

    ```

### State 的更新会被合并

> 当你调用 setState() 的时候，React 会把你提供的对象合并到当前的 state。

```js
    // 例如，你的 state 包含几个独立的变量：

    constructor(props) {
        super(props);
        this.state = {
            posts: [],
            comments: []
        };
    }

    // 然后你可以分别调用 setState() 来单独地更新它们：

    componentDidMount() {
        fetchPosts().then(response => {
            this.setState({
                posts: response.posts
            });
        });

        fetchComments().then(response => {
            this.setState({
                comments: response.comments
            });
        });
    }

    // 这里的合并是浅合并，所以 this.setState({comments}) 完整保留了 this.state.posts， 但是完全替换了 this.state.comments。

```


### 列表 & Key

1. 在 map() 方法中的元素需要设置 key 属性。
2. key 只是在兄弟节点之间必须唯一


### 组合 vs 继承

1. React 中没有“槽”（slot）这一概念的限制，你可以将任何东西作为 props 进行传递。

2. Props 和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意 props，包括基本数据类型，React 元素以及函数。
