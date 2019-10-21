---
title: vuecli3.0配置实践
categories: vue vuecli
tags: webpack
keywords: [vue-cli, webpack]
description: vuecli3.0基础配置
---
# vuecli3.0一些基础配置

使用vue-cli3生成项目之后的基础配置
#### 1.首先在跟目录新建几个文件
配置文件 vue.config.js
环境变量文件
.env.local
.env.pre
.env.dev
.env.prod

#### 2.修改router/router.js文件的base属性

```
base: '/'
```
如果不修改的话   上线之后会在每个路由前面加上publicPath

#### 3.配置postCss 问css属性加浏览器前缀，在根目录下的postcss.config.js

```
module.exports = {
  plugins: {
    autoprefixer: {
      overrideBrowserslist: [
        "Android 4.1",
        "iOS 7.1",
        "Chrome > 31",
        "ff > 31",
        "ie >= 8"
      ]} 
}}
```

#### 4.配置项目 在vue.config.js

```
const path = require('path')

function resolve (dir) {
  return path.join(__dirname, dir)
}
const config = {
  common: {
    assetsDir: 'static'
  },
  local: {
    baseUrl: '/',
    devServer: {
      disableHostCheck: true
    }
  },
  mock: {
    baseUrl: 'xxx',
    devServer: {
      proxy: {
        '/mock': {
          'target': 'http://60.205.222.230:3000/mock/',
          'pathRewrite': {
            '^/mock': ''
          }
        }
      }
    }
  },
  dev: {
    baseUrl: 'xxx',
    devServer: {
      disableHostCheck: true
    }
  },
  pre: {
    baseUrl: 'https://cdn.frxing.cn/xxx/'
  },
  prod: {
    // baseUrl: 'http://127.0.0.1:8088/'
    // 这个是你的cdn地址   也可以是项目所在的域名地址加目录
    baseUrl: 'https://cdn.frxing.cn/xxx/' 
  }
}

module.exports = {
  assetsDir: 'static',
  publicPath: config[process.env.CURR_ENV].baseUrl,
  lintOnSave: false,
  chainWebpack: (config) => {
    config.resolve.alias
      .set('@', resolve('src'))
// 此配置要先安装image-webpack-loader
   config.module
   .rule('images')
    .use('image-webpack-loader')
   .loader('image-webpack-loader')
   .options({
        bypassOnDebug: true
    }).end()

    config.output.filename('[name].[hash].js').end()
  }
}
```

#### 4.项目配置webpack-bundle-analyzer

在vue-cli2.0的时候 自带了分析工具  
执行下面命令就可以看到

```
npm run build --report
```

但是在vue-cli3.0的时候需要我们去配置
首先

```
npm install webpack-bundle-analyzer -D
```
然后在vue.config.js中增加配置

```
chainWebpack: (config) => {
    // 判断是否执行的build命令
    if (process.env.NODE_ENV === 'production') {
      // 判断是否有report参数
      if (process.env.npm_config_report) {
        config
          .plugin('webpack-bundle-analyzer')
          .use(require('webpack-bundle-analyzer').BundleAnalyzerPlugin).end()
        config.plugins.delete('prefetch')
      }
    }
  }
```
这里需要注意的是如果你的项目使用了本地的环境变量 例：.env.production   需要在对应的环境变量的文件中重写NODE_ENV,如果不重写在执行有--mode的时候所有的NODE_ENV都会是development。
这样在执行 npm run build --report 就可以打开分析工具了