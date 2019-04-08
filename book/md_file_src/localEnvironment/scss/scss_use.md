#scss use

##编译sass

* sass编译有很多种方式，如命令行编译模式、sublime插件SASS-Build、编译软件koala、前端自动化软件codekit、Grunt打造前端自动化工作流grunt-sass、Gulp打造前端自动化工作流gulp-ruby-sass等。

###命令行编译;

```
    //单文件转换命令(scss 目标scss文件  生成的css文件)
    sass input.scss output.css

    //单文件监听命令
    sass --watch input.scss:output.css

    //如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录(scss --watch 监测目录:生成目录)：
    sass --watch app/sass:public/stylesheets
```

####** 注意：监测的目录不宜过深，最好可以用相对路径作为监测目录路径，有效的加快监测效率。**

###命令行编译配置选项;
>命令行编译`sass`有配置选项，如编译过后`css`排版、生成调试`map`、开启`debug`信息等，可通过使用命令`sass -v`查看详细。我们一般常用两种`--style''` `--sourcemap`。

```
    //编译格式
    sass --watch input.scss:output.css --style compact

    //编译添加调试map
    sass --watch input.scss:output.css --sourcemap

    //选择编译格式并添加调试map
    sass --watch input.scss:output.css --style expanded --sourcemap

    //开启debug信息
    sass --watch input.scss:output.css --debug-info
```

* `--style`表示解析后的`css`是什么排版格式;   
* `sass`内置有四种编译格式: `nested`、`expanded`、`compact`、`compressed`。   
* `--sourcemap`表示开启`sourcemap`调试。开启`sourcemap`调试后，会生成一个后缀名为`.css.map`文件。   

##四种编译排版演示;

###未编译样式
```css

.box {
  width: 300px;
  height: 400px;
  &-title {
    height: 30px;
    line-height: 30px;
  }
}

```
>nested 编译排版格式

```css

/*命令行内容*/
sass style.scss:style.css --style nested

/*编译过后样式*/
.box {
  width: 300px;
  height: 400px; }
  .box-title {
    height: 30px;
    line-height: 30px; }

```

>expanded 编译排版格式

```css

/*命令行内容*/
sass style.scss:style.css --style expanded

/*编译过后样式*/
.box {
  width: 300px;
  height: 400px;
}
.box-title {
  height: 30px;
  line-height: 30px;
}

```

>compact 编译排版格式

```css

/*命令行内容*/
sass style.scss:style.css --style compact

/*编译过后样式*/
.box { width: 300px; height: 400px; }
.box-title { height: 30px; line-height: 30px; }

```

>compressed 编译排版格式


```css

/*命令行内容*/
sass style.scss:style.css --style compressed

/*编译过后样式*/
.box{width:300px;height:400px}.box-title{height:30px;line-height:30px}

```

##简单计算公式

1. 动态计算百分比

    ```scss

        $dw:750;            //设计图的宽度

        @function percent($px){
            @return $px/$dw*100%;
        }
        
        .logo {
            width: percent(100);  //动态计算百分比宽
        }

    ```

2. 设计图尺寸除2，求出移动端的尺寸

    ```scss

        @function d($px){
            @return $px/2;
        }

        .logo {
            width: d(100px);  //动态计算宽
        }

    ```

3. 动态计算rem

    ```scss

        $font-size : 20px;  //页面的根字体大小

        @function r($px){
            @return $px/$font-size*1rem;
        }

    ```




>附上：[scss官方文档](https://www.sass.hk/guide/)