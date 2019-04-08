#Gitbook 安装使用
----------
##introduce
* 主要用来书写程序说明文档，或自己的学习资料，所以是非常有用		
* Gitbook 是基于 Node.js 的命令行工具，用来创建漂亮的电子书，它使用 Markdown 或 AsciiDoc 语法来撰写内容，用 Git 进行版本控制，且可以托管在 Github 上。Gitbook 可以将作品编译成网站、 PDF、 ePub 和 MOBI 等多重格式。		
* 如果你不擅长自己搭建 gitbook 环境，还可以使用 [gitbook.com](https://www.gitbook.com/) 在线服务来创建和托管你的作品，他们还提供了基于桌面的 [编辑器](https://www.gitbook.com)。

##install
####首先在全局安装 gitbook 客户端工具:   		

```
	npm install gitbook-cli -g
	
```   


####创建book.json并安装 创建一个空的文件夹，并在这个文件夹中创建一个book.json文件

```

	{
		"gitbook":"2.x.x"
	}
	
```

>表示安装gitbook的2.x.x版本，以后可以扩展此文件安装更多的gitbook插件
	
```
	gitbook install
```
	
####创建README.md 和 SUMMARY.md
```
	gitbook init
```
>在你的作品目录中创建两个必需的文件 README.md 和 SUMMARY.md，README.md 是作品的介绍，SUMMARY.md 是作品的目录结构，里面要包含一个章节标题和文件索引的列表：

```
		# Summary
		
		This is the summary of my book.
	
		* [section 1](section1/README.md)
			* [example 1](section1/example1.md)
			* [example 2](section1/example2.md)
				- [example 3](section1/example3.md)
		* [section 2](section2/README.md)
			* [example 1](section2/example1.md)
	
```


####运行服务，在编辑内容后实时预览:

```
		gitbook serve  
```
	
>浏览器打开 [http://localhost:4000](http://localhost:4000) 查看
	
	
####撰写完后可以生成静态网站用来发布:

```
	gitbook build
```
	
####更多命令帮助

```
	gitbook help
```
	
####自由切换版本

```
	gitbook ls-remote           //罗列出所有的版本号
	gitbook fetch 2.1.0         //选择其中一个版本切换安装
```
	
####想要添加gitbook配置或插件，只需要在book.json里面添加即可

+ 示例
```
		{
		"gitbook" : "2.x.x",
		"title":"NoteBook",
		"author":"Liujw",
		"plugins":[
			"sectionx",
			"expandable-chapters"
		]
	}
```










