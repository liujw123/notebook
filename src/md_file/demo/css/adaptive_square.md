# 自适应正方形   

### *关键 padding（%） 	规定基于父元素的宽度的百分比的内边距。*   

``` html
    <!doctype html>
    <html>
        <head>
            <meta charset="utf-8">
            <title>title</title>
            <meta name="Robots" content="All" />
            <meta name="viewport" content="initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,width=device-width, user-scalable=no, minimal-ui">
            <meta content="yes" name="apple-mobile-web-app-capable">
            <meta content="black" name="apple-mobile-web-app-status-bar-style">
            <meta content="telephone=no, email=no" name="format-detection">
            <style>
                .ul {font-size:0;padding:5px; }
                .ul li { display: inline-block; vertical-align: top; width: 33.3%; padding-top: 33.3%;position:relative; }
                .ul div { position: absolute;left:0;top:0; width: 100%; height: 100%; padding:5px; }
                .ul p { display: block; width: 100%; height: 100%; position: relative;background-color:#000; }
                .ul span { position: absolute; left: 50%; top: 50%; transform: translate(-50%,-50%); -moz-transform: translate(-50%,-50%); -webkit-transform: translate(-50%,-50%); -o-transform: translate(-50%,-50%); -ms-transform: translate(-50%,-50%);
                        font-size:12px;color:#fff;line-height:18px;text-align:center;
                }
            </style>
        </head>
        <body>
            <ul class="ul">
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
                <li>
                    <div> <p> <span> texttexttext </span> </p> </div>
                </li>
            </ul>
        </body>
    </html> 

```

