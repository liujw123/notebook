###Vuex 简单配置使用

####Vuex简介

>vuex是一个专门为vue.js设计的集中式状态管理架构。状态？我把它理解为在data中的属性需要共享给其他vue组件使用的部分，就叫做状态。简单的说就是data中需要共用的属性。


#####1. 安装 vuex(加上 –save，因为你这个包我们在生产环境中是要使用的。)

```js
	 npm install vuex --save
```
	
#####2.新建一个store文件夹（这个不是必须的），并在文件夹下新建*index.js*文件，文件中引入我们的vue和vuex。

> 使用vuex，引入之后用Vue.use进行引用。   

> 用export default 封装代码，让外部可以引用。

	
```js
	//index.js
	
	import Vue from 'vue';
	import Vuex from 'vuex';
	
	Vue.use(Vuex);
	
	export default new Vuex.Store({
        state:{
        	count:1
        },
        mutations:{
        	add(state){
	            state.count++;
	        },
	        reduce(state){
	            state.count--;
	        }
        }
    });
```

#####3. 在*main.js* 中引入新建的vuex文件, 在实例化 Vue对象时加入 store 对象 :

```js
	//main.js
	
	import storeConfig from './store'
	
	new Vue({
      el: '#app',
      router,
      store,//使用store
      template: '<App/>',
      components: { App }
    })
```


#####4. 组件引用

```vue

	<template>
	    <div class="container">
	    	<button @click="$store.commit('reduce')">-</button>
	        <span>{ {$store.state.count} }</span>
	        <button @click="$store.commit('add')">+</button>
	    </div>
	</template>
	
```

>搞定，附上：[vuex文档](https://vuex.vuejs.org/zh/)
