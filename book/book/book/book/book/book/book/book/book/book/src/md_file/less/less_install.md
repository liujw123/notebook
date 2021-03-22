## less 环境搭建

### 1. 首先搭建[node](../node/less_install.md)环境;   

#### 验证node   

```js
    node -v // 检测 node 版本
    npm -v  // 检测 npm 版本
```

-----

### 2. 全局安装 less 编译模块 [lessc](http://lesscss.org/);   

```
    npm install -g less // 全局安装less编译模块
    lessc -v   // 检测 less 模块是否安装成功
```

#### 使用  

> 切换到项目目录，输入以下命令：

```
    lessc test.less > test.css
    // lessc 入口文件路径 > 输出文件路径 

```

----

### 3. 全局安装 less 监听模块 [watcher-lessc](https://www.npmjs.com/package/watcher-lessc);

1. 指定@import指令的搜索路径。
2. 压缩CSS输出。
3. 指定文件名，以获得更好的错误消息。
4. 指定要监视的输入目录。

```
    npm install -g watcher-lessc // 全局安装less实时监听编译模块

```

#### 使用   

##### 说明

| Options | 命令 | 说明 | 大意 |
| :-: | :-: | :-: | :-: |
| --input | -i | Specify input file to watch/compile. | 指定监听或编译入口文件 |
| --paths | -p | Specify search paths for @import directives.    [default: []]. | 指定@import指令的搜索路径 |
| --compress | -c | Minify CSS output. | 输出压缩css |
| --filename | -f | Specify a filename, for better error messages. [default: "style.less"] | 指定一个文件名，以更好的获取错误信息 |
| --directory | -d | Specify input directory to watch. | 要监视的特定输入目录 |
| --output | -o |Specify output file path. | 指定输出文件路径 |
| --help | -h | Show this message. | 显示此消息 |

> 使用命令

```
    // 1. Specify input .less file & output .css file
    watcher-lessc -i less/index.less -o css/indexcss

    // 2. Specify input directory to watch.
    watcher-lessc -i less/index.less -o css/index.css -d ./less

    // 3. Specify less render options
    watcher-lessc -i less/index.less -o css/index.css -d ./less -p ./less -f style.less -c
```


-----

> [less中文文档](http://lesscss.cn/)