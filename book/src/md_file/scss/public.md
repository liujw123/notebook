# 公共样式（例子）

``` scss

    @charset "utf-8";

    $themeColor: #009874;

    /*每个页面公共css */
    /* view,scroll-view,text,picker{
    box-sizing: border-box;
    } */

    @mixin textHide($line) {
        display: -webkit-box;
        word-break: break-all;
        text-overflow: ellipsis;
        overflow: hidden;
        -webkit-box-orient: vertical;
        -webkit-line-clamp:$line;
    }

    @mixin centerFlex($justtify){
        display: flex;
        align-items: center;
        justify-content: $justtify;
    }

    @mixin botLine{
        border-bottom: 2upx solid #e5e5e5;
    }

    @mixin picBgc{
        background-color: #f9f9f9;
    }


    @mixin closeIcon($width,$height,$color) {
        transform: rotateZ(45deg);
        &::before{
            content: '';
            display: block;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%,-50%);
            width: $width;
            height: $height;
            background-color: $color;
            border-radius: $height;
        }
        &::after{
            content: '';
            display: block;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%,-50%);
            width: $height;
            height: $width;
            background-color: $color;
            border-radius: $height;
        }
    }

    @mixin hover {
        position: relative;
        overflow: hidden;
        &::after{
            content: '';
            position: absolute;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            z-index: 2;
            background-color: rgba(0,0,0,.1);
        }
    }

    @mixin arrowIcon($size,$line,$radius,$angle,$color) {
        &::after{
            content:'';
            display: inline-block;
            vertical-align: middle;
            transform: rotateZ($angle);
            width: $size;
            height: $size;
            border-right: $line solid $color;
            border-bottom: $line solid $color;
            border-bottom-right-radius: $radius;
        }
    }


```