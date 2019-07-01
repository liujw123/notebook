###vue-scss 配置使用

####1. 安装对应依赖node模块：

```js
	npm install node-sass --save-dev
	npm install sass-loader --save-dev
```
	
####2. 打开webpack.base.config.js在loaders里面加上

```js
	rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
  +---
      {
        test: /\.scss$/,
        loaders: ["style", "css", "sass"]
      },
  +---
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }
```

####3. 在用scss的地方写上

```html
	<style lang="scss" scoped="" type="text/css"></style>
```


>附上：[scss文档](https://www.sass.hk/docs/)
