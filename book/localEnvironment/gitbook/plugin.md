#Gitbook 插件
----------



####Gitbook默认带有5个插件： 

* highlight 
* search  
* sharing  
* font-settings 
* livereload    
>+ 如果要去除自带的插件， 可以在插件名称前面加"-"   

```json
	"plugins": [
	    "-search"
	]
```

###title

* 设置书本的标题

```json

	"title" : "Gitbook Use"

```

###author

* 作者的相关信息

```json

	"author" : "zhangjikai"

```


###description

* 本书的简单描述

```json

	"description" : "记录Gitbook的配置和一些插件的使用"

```


###language

* Gitbook使用的语言, 版本2.6.4中可选的语言如下：

```
	en, ar, bn, cs, de, en, es, fa, fi, fr, he, it, ja, ko, no, pl, pt, ro, ru, sv, uk, vi, zh-hans, zh-tw
```


* 配置使用简体中文

```json

	"language" : "zh-hans",

```



###links

* 在左侧导航栏添加链接信息

```json

	"links" : {
	    "sidebar" : {
	        "Home" : "http://zhangjikai.com"
	    }
	}

```


###styles

* 自定义页面样式， 默认情况下各generator对应的css文件

```json

	"styles": {
	    "website": "styles/website.css",
	    "ebook": "styles/ebook.css",
	    "pdf": "styles/pdf.css",
	    "mobi": "styles/mobi.css",
	    "epub": "styles/epub.css"
	}

```

例如使`<h1> <h2>`标签有下边框， 可以在website.css中设置

```

	h1 , h2{
	    border-bottom: 1px solid #EFEAEA;
	}

```


###plugins
* 配置使用的插件

```json

	"plugins": [
	    "disqus"
	]

```


##**实用插件**

####添加新插件之间需要运行gitbook install来安装新的插件

#####[expandable Chapters](https://plugins.gitbook.com/plugin/expandable-chapters)

* 左侧菜单折叠

```json

	{
	    plugins: ["expandable-chapters"]
	}

```

#####[Disqus](https://plugins.gitbook.com/plugin/disqus)

* 添加disqus评论

```json

	"plugins": [
	    "disqus"
	],
	"pluginsConfig": {
	    "disqus": {
	        "shortName": "gitbookuse"
	    }
	}

```

#####[search Pro](https://plugins.gitbook.com/plugin/search-pro)
* 支持中文搜索, 需要将默认的search插件去掉, 注意: 如果标题中有包含的关键字, 标题的样式会有所变化 

```json

	"plugins": [
	    "-search",
	    "search-pro"
	],
	
	"pluginsConfig": {
	    "search-pro": {
	        "cutWordLib": "nodejieba",
	        "defineWord" : ["Gitbook Use"]
	    }
	}

```

#####[to top](https://plugins.gitbook.com/plugin/back-to-top-button)

* 返回顶部

```json
	"plugins": [
	    "back-to-top-button"
	],
```

#####[Github](https://plugins.gitbook.com/plugin/github)

* 添加github图标 

```json	
	"plugins": [ 
	    "github" 
	],
	"pluginsConfig": {
	    "github": {
	        "url": "https://github.com/liujw123"
	    }
	}
```

#####[Splitter](https://plugins.gitbook.com/plugin/splitter)

* 使侧边栏的宽度可以自由调节

```json

	"plugins": [
	    "splitter"
	]

```

#####[Sectionx](https://plugins.gitbook.com/plugin/sectionx)

* 将页面分块显示 

```json

	"plugins": [
	   "sectionx"
	]

```

#####[Tbfed-pagefooter](https://plugins.gitbook.com/plugin/tbfed-pagefooter)

* 为页面添加页脚 

```json

	"plugins": [
	   "tbfed-pagefooter"
	],
	"pluginsConfig": {
	    "tbfed-pagefooter": {
	        "copyright":"Copyright &copy zhangjikai.com 2015",
	        "modify_label": "该文件修订时间：",
	        "modify_format": "YYYY-MM-DD HH:mm:ss"
	    }
	}

```


#####[Toggle Chapters](https://plugins.gitbook.com/plugin/toggle-chapters)

* 是左侧的章节目录可以折叠 

```json

	"plugins": ["toggle-chapters"]

```


#####[ga](https://plugins.gitbook.com/plugin/ga)

* google 统计 

```json

	"plugins": [
	    "ga"
	 ],
	"pluginsConfig": {
	    "ga": {
	        "token": "UA-XXXX-Y"
	    }
	}

```


#####[3-ba](https://plugins.gitbook.com/plugin/3-ba)

* 百度统计 

```json

	{
		"plugins": ["3-ba"],
		"pluginsConfig": {
			"3-ba": {
				"token": "xxxxxxxx"
			}
		}
	}

```