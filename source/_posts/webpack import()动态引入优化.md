---
title: webpack import()动态引入优化
categories: vue
tags: webpack
keywords: [vue-cli, webpack]
description: 解决异步加载，build出来的chunk文件都是已id命名的问题
---

#### 1.将组件改异步加载
首先组件需要异步，并且在import方法内加上注释webpackChunkName: "filename"(注：filename名字一定要用引号包起来否则是不生效的) 如果出现两个以上异步组件的webpackChunkName一样，那么这两个组件就打包到一个js文件内

```
// 以vue-router为例

{
  path: '/home',
  name: 'Home',
  component: () => import(/* webpackChunkName: "Home" */ './Home')
}
```

#### 2. webpack打包配置
vue-cli默认chunkFilename取的是id，改为name

```
output: {
    path: config.build.assetsRoot,
    filename: utils.assetsPath('js/[name].[chunkhash].js'),
    chunkFilename: utils.assetsPath('js/[name].[chunkhash].js')
}
```

#### 3. 其它
一般情况1，2配置完成以后重新build文件名字就会变成以name命名的，但是我司用的vue-cli生成的模版，还需要修改.babelrc配置文件

```
{
...
 "comments": true // 默认为false，false会把所有的注释都编译掉
...
}
```