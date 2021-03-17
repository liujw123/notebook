# scss 文件对应目录监听编译

*只能单文件监听，公共scss编译后，暂时无法触发全scss文件重现编译*

```js 

    // 针对性输出文件，目前配置只能对单文件进行监听输出
    // 多数插件只能用cnpm安装，安装插件时切换淘宝镜像

    var gulp = require("gulp");                         // 定义任务
    var gulpPlumber = require("gulp-plumber");          // 禁止报错终止监听，弹出暴露信息
    var gulpSass = require("gulp-sass");                // sass转换压缩
    var gulpRename = require("gulp-rename");            // 重命名，定义详细输出路径
    var debug = require('gulp-debug');                  // 输出调试信息
    var autoprefixer = require('gulp-autoprefixer');    // css兼容前缀添加

    var PROJECT_NAME = 'SquashWX'                       // 要监听的项目
    var OUTPUT_DIRNEME = 'subpackages';                 // 输出文件的上级dir所在目录  // subpackages pages  components
    var OUT_PUT_FILE_EXTNAME = '.wxss'                  // 输出文件格式
    var SCSS_FOLDER_NAME = 'scss'                       // scss 存放的目录名，映射 scss 目录结构进行输出

    // scss 里面的路径
    function getScssFilePath(pathString){
        return pathString.substring(pathString.indexOf(`${SCSS_FOLDER_NAME}\\`)+5,pathString.indexOf('.'));
    }

    // 监听scss
    gulp.task("watchscss",function(){
        gulp.watch(`./${PROJECT_NAME}/**/*.scss`).on('change',function(path){
            var fileName = getScssFilePath(path).substring(getScssFilePath(path).indexOf('\\')+1);
            if( fileName == 'public' || fileName == 'style' || fileName == 'static')return;
            gulp.src(path)
            .pipe(debug())
            .pipe(gulpPlumber())
            .pipe(gulpSass({
                outputStyle: 'compact'
            }))
            .pipe(autoprefixer([
            'iOS >= 8',
            'Android >= 4.1'
            ]))
            .pipe(gulpRename({
                dirname: getDirnameRule(OUTPUT_DIRNEME,path,fileName),
                extname: OUT_PUT_FILE_EXTNAME
            }))
            .pipe(gulp.dest(`./${PROJECT_NAME}/${OUTPUT_DIRNEME}`))
            .pipe(debug())
        })
    })


    function getDirnameRule(OUTPUT_DIRNEME,path,fileName){
        if(OUTPUT_DIRNEME == 'pages') return getScssFilePath(path);                                              // 主包层级映射
        if(OUTPUT_DIRNEME == 'subpackages') return `${getScssFilePath(path).replace('\\','\\pages\\')}`;    // 分包时对页面独立存放映射
        return fileName;                                                                                    // 其余直接映射
    }





```