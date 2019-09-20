###npm
####npm命令
```js

	npm -v          #显示版本，检查npm 是否正确安装。
	npm root -g     //获取当前node_modeuls全局插件的安装位置，其实就是prefix变量后面追加node_modules
	npm install express   #安装express模块
	npm install express@3.5.0   #安装指定版本的express模块
	npm install -g express  #全局安装express模块
	npm uninstall express (-g)  #删除指定的模块
	npm update        #升级当前目录下的项目的所有模块
	npm update webpack    #升级当前目录下的项目的指定模块
	npm update webpack@latest    #更新当前目录的webpack模块到最新版
	npm update webpack@2.5.0    #更新指定版本的webpack模块到2.5.0版本
	npm update -g express  #升级全局安装的express模块
	npm list      #列出已安装模块，可以缩写为npm ls,如需查看全局则加-g参数
	npm list  --depth=0 -g   #列出已安装的全局高级模块，该命令常用来检查本地是否安装某个应用库
	npm config set prefix c:\123\   #设置配置，例如设置全局node_module所在文件夹
	npm config get prefix           #读取配置，例如读取全局node_module所在文件夹
	
	
	npm show express     #显示模块详情
	npm cache clean 清理缓存
	npm help xxx  查看帮助
	npm view moudlename dependencies  查看包的依赖关系
	npm view modulenames  查看node模块的package.json文件夹
	npm view modulename labelname  查看package.json文件夹下某个标签的内容
	npm view modulename repository.url  查看包的源文件地址
	npm view modulename engines   查看包所依赖的node的版本
	npm help folders   查看npm使用的所有文件夹
	npm rebuild modulename    用于更改包内容后进行重建
	npm outdated   检查包是否已经过时，此命令会列出所有已经过时的包，可以及时进行包的更新
	
	## 关于发布模块的几个常用命令
	npm adduser                 //注册https://www.npmjs.com 上面的账号
	npm login                //用注册的账户登录，需要输入账户名，密码和邮箱
	npm owner ls <package_name> //查看模块拥有者 
	npm owner add <user> <package_name>  //添加一个发布者 
	npm owner rm <user> <package_name> //删除一个发布者 
	npm publish                 //把自己的NPM包推送到npmjs市场上
	npm unpublish 包名 --force   //删除在npmjs市场上的包，但注意，现在官方规定只能删除24小时内发布的包
	npm search <package_name>   //搜索市场是是否存在某个包名

```


####--save-dev 与 --save区别

> --save-dev 是你开发时候依赖的东西   
> --save 是你发布之后还依赖的东西
>>比如，你写 ES6 代码，如果你想编译成 ES5 发布那么 babel 就是devDependencies。 如果你用了 jQuery，由于发布之后还是依赖jQuery，所以是dependencies。


* 正常使用npm install时，会下载dependencies和devDependencies中的模块，当使用npm install --production或者注明NODE_ENV变量值为production时，只会下载dependencies中的模块。

####关于本地安装和全局安装后
* 全局安装
> `npm i -g webpack` 这样是可以在命令行直接使用的(因为添加了环境变量，可以直接在命令行中找到该命令)。 但弊端就是所有项目的webpack版本都依赖于全局的webpack当前版本，无法做到差异化。


* 局部安装
> `npm i webpack --save-dev`		


######可以通过下面两种形式来调用局部命令
1. 补全本地命令路径来调用

	```js
		./node_modules/.bin/webpack --help #linux或mac环境下
		 
	 	.\node_modules\.bin\webpack --help #windows环境下
	```
	因为本地安装的命令，没有加入到环境变量中，无法直接调用，需要补全路径
	
2. 直接写在package.json的script脚本里   

	```js
		//package.json
		   "scripts": {
		     "webpack-v":"webpack -version"
		   },
	```
	
	命令行调用
	
	```
		 npm run webpack-v
	```	
	
	这里运行的webpack命令，会优先从本地项目中的node_modules里面找
