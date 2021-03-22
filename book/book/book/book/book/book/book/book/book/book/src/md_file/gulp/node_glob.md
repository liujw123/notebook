# Gulp 文件路径匹配规则 - node-glob   

> Gulp使用 [node-glob](https://github.com/isaacs/node-glob) 模块，借助 [minimatch](https://github.com/isaacs/minimatch) 库，将glob表达式(glob expressions)转换成JavaScript正则表达式(JavaScript RegExp) ，从而实现文件匹配功能。   

### 语法：    

1. ` * ` 匹配文件路径中的0个或多个字符，但不会匹配路径分隔符，除非路径分隔符出现在末尾。   
2. ` ** ` 匹配路径中的0个或多个目录及其子目录，需要单独出现，即它左右不能有其他东西了。如果出现在末尾，也能匹配文件。   
3. ` ? ` 匹配文件路径中的一个字符(不会匹配路径分隔符)。   
4. `[...]` 匹配方括号中出现的字符中的任意一个，当方括号中第一个字符为 ^ 或 !时，则表示不匹配方括号中出现的其他字符中的任意一个。   
5. ` !(pattern|pattern|pattern) ` 匹配任何与括号中给定的任一参数一致的都不匹配的。   
6. ` ?(pattern|pattern|pattern) ` 匹配括号中给定的任一参数0次或1次。   
7. ` +(pattern|pattern|pattern) ` 匹配括号中给定的任一参数至少1次。   
8. ` *(a|b|c) ` 匹配括号中给定的任一参数0次或多次。   
9. ` @(pattern|pat*|pat?erN) ` 匹配括号中给定的任一参数1次。   


----

### 实例：

1. ` * ` 能匹配 `a.js` , `x.y` , `abc` , `abc/` ，但不能匹配 `a/b.js`   
2. ` *.* ` 能匹配 `a.js` , `style.css` , `a.b` , `x.y`    
3. ` */*/*.js ` 能匹配 `a/b/c.js` , `x/y/z.js` ，不能匹配 `a/b.js` , `a/b/c/d.js`    
4. ` ** ` 能匹配 `abc` , `a/b.js` , `a/b/c.js` , `x/y/z` , `x/y/z/a.b` ，能用来匹配所有的目录和文件
5. ` **/*.js ` 能匹配 `foo.js` , `a/foo.js` , `a/b/foo.js` , `a/b/c/foo.js`    
6. ` a/**/z ` 能匹配 `a/z` , `a/b/z` , `a/b/c/z` , `a/d/g/h/j/k/z`    
7. ` a/**b/z ` 能匹配 `a/b/z` , `a/sb/z` ，但不能匹配 `a/x/sb/z` ，因为只有单 `**`单独出现才能匹配多级目录   
8. ` ?.js ` 能匹配 `a.js` , `b.js` , `c.js`    
9. ` a?? ` 能匹配 `a.b` , `abc` ，但不能匹配 `ab/` ，因为它不会匹配路径分隔符   
10. ` [xyz].js ` 只能匹配 `x.js` , `y.js` , `z.js` ，不会匹配 `xy.js` , `xyz.js` 等，整个中括号只代表一个字符   
11. ` [^xyz].js ` 能匹配 `a.js` , `b.js` , `c.js` 等，不能匹配 `x.js` , `y.js` , `z.js`   

---

> 当有多种匹配模式时可以使用数组：   

```js
    // 使用数组的方式来匹配多种文件
    gulp.src(['js/*.min.js','sass/*.min.css'])
```

> 使用数组的方式还有一个好处就是可以很方便的使用排除模式，在数组中的单个匹配模式前加上 ! 即是排除模式，它会在匹配的结果中排除这个匹配，要注意一点的是不能在数组中的第一个元素中使用排除模式：  

```js
    // 使用数组的方式来匹配多种文件
    gulp.src(['*.js','!b*.js']) // 匹配所有js文件，但排除掉以b开头的js文件
    gulp.src(['!b*.js',*.js]) // 不会排除任何文件，因为排除模式不能出现在数组的第一个元素中
```

> 此外，还可以使用展开模式。展开模式以花括号作为定界符，根据它里面的内容，会展开为多个模式，最后匹配的结果为所有展开的模式相加起来得到的结果。展开的例子如下：   

``` js
    a{b,c}d 会展开为 abd,acd 
    a{b,}c 会展开为 abc,ac 
    a{0..3}d 会展开为 a0d , a1d , a2d , a3d 
    a{b,c{d,e}f}g 会展开为 abg , acdfg , acefg 
    a{b,c}d{e,f}g 会展开为 abdeg , acdeg , abdeg , abdfg
```

--- 