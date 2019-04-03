---
title: vue-cli构建的vue项目全局引入scss的mixin方法
categories: vue
tags: vue
keywords: [webpack, vue]
description: 在vue项目中集成了sass，但是如果想用mixin等方法的时候，要在每个.vue文件的style标签里面引入@import "../src/assets/css/mixin.scss"，次篇文章就是为了解决这个通点
---
vue-cli构建的vue项目全局引入scss的mixin方法

1、首先安装模块

```
npm install sass-resources-loader --save-dev
```
2、在webpack中进行配置
在build文件夹目录下找到util.js文件，在该文件下找到exports.cssLoaders方法，在方法中加入如下代码

```
var sassResourceLoader = {
    loader: 'sass-resources-loader',
    options: {
      resources: [
        path.resolve(__dirname, '../src/assets/css/mixin.scss'),
      ]
    }
  }
```
3、修改generateLoaders的方法增加参数anotherLoader

```
function generateLoaders (loader, loaderOptions, anotherLoader) {
    var loaders = [cssLoader, px2rpxLoader, postcssLoader]

    if (loader) {
      loaders.push({
        loader: loader + '-loader',
        options: Object.assign({}, loaderOptions, {
          sourceMap: options.sourceMap
        })
      })
    }
    if (!!anotherLoader) loaders.push(anotherLoader)
    // Extract CSS when that option is specified
    // (which is the case during production build)
    if (options.extract) {
      return ExtractTextPlugin.extract({
        use: loaders,
        fallback: 'vue-style-loader'
      })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }
  }
```
其实就是加了if (!!anotherLoader) loaders.push(anotherLoader)这句话
4、修改return返回的对象里面的sass和scss

```
{
    ...
    sass: generateLoaders('sass', { indentedSyntax: true}, sassResourceLoader),
    scss: generateLoaders('sass', {}, sassResourceLoader),
    ...
}
```
这样的话就无需在每个.vue文件的style标签前面写
`@import "../src/assets/css/mixin.scss"`


