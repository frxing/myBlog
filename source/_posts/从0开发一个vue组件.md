---
title: 从0开发一个vue组件库
categories: javascript
tags: javascript
keywords: [vue, javascript]
description: 从0开发一个vue组件库,类似于element、iview等
---
# 从0开发一个vue组件库

先打个广告，前几天撸了一个vue画板组件，使用的fabric.js做的，
项目地址：[https://github.com/frxing/fabric-sketchpad](https://github.com/frxing/fabric-sketchpad)
预览地址：http://sketchpad.frxing.cn
欢迎star
欢迎指正
使用vuecli3来快速搭建一个属于自己的组件库

1. 使用vuecli3创建项目
   
   ```
   vue create fabric-sketchpad
   ```

2.    更改项目的目录，把原来的src目录改成examples, 并在同级新建packages目录用于存放我们的组件源码
   
3. cli默认回编译src目录，所以更改我们的编译打包文件，将packages目录加入编译
   
   ```
   module.exports = {
     pages: {
       index: {
         entry: 'examples/main.js',
         template: 'public/index.html',
         filename: 'index.html'
       }
     },
     chainWebpack: config => {
       config.module
         .rule('js')
         .include
           .add('/packages')
           .end()
         .use('babel')
           .loader('babel-loader')
     }
   }
   ```

4. 在packages目录下新建一个我们的组件目录sketchpad,sketchpad下面新建一个src目录和一个index.js文件
   
  ```javascript
  |--packages
  |--|--sketchpad
  |--|--|--src
  |--|--|--|--main.vue
  |--|--|--|--index.scss
  |--|--|--index.js
  ```
   src里面的vue文件写我们的组件，index.scss写组件的样式然后在vue组件里面把index.scss引入，在sketchpad下面的js里面把组件导入
   ```javascript
  // 导入组件，组件必须声明 name
  import Sketchpad from './src/main'
  // 为组件提供 install 安装方法，供按需引入
  Sketchpad.install = function (Vue) {
    Vue.component(Sketchpad.name, Sketchpad)
  }
  // 导出组件
  export default Sketchpad   
  ```
5. 在examples下面的main.js里面引入组件测试
   
   ```javascript
   import vueFabricSketchpad from '../packages/sketchpad'
   
   Vue.use(vueFabricSketchpad)
   ```
6. 配置package.json文件
   
   ```json
   "version": "0.1.0",
     "main": "lib/vue-fabric-sketchpad.common.js",
     "scripts": {
       "serve": "vue-cli-service serve",
       "build": "vue-cli-service build",
       "lint": "vue-cli-service lint",
       "lib": "vue-cli-service build --target lib --name vue-fabric-sketchpad --dest lib packages/sketchpad/index.js"
     },
   ```
7. 在根目录增加.npmignore文件
   
   ```javascript
   examples/
   packages/
   public/
   dist/
   node_modules/
   
   # 忽略指定文件
   vue.config.js
   babel.config.js
   *.map
   ```
8. 发布npm包
   
   ```javascript
   npm login 
   ...
   ...
   ...
   npm publish
   ```

这里注意每次发包要更改package.json里面的版本号，否则发不失败
