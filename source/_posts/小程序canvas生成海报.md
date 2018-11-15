---
title: 小程序 - 使用小程序的canvas把多张图片合成一张
categories: 小程序
tags: 小程序
keywords: [小程序]
description: 小程序是不能够分享到朋友圈的，所以很多公司都想让生成一张带有小程序码的图片，让用户分享到朋友圈，从而达到用户拉新的目的
---

前几天接到需求要做一个小程序的项目，由于任务比较急，所以我用的是 mpVue 框架来开发小程序（公司前面的小程序项目都是用的 mpvue）。

在这个需求里面其中有一个功能是要求生成一张带有用户头像和带有一个小程序码的海报，下面我把我的实现思路写下来。

首先在 data 里面定义一个字段用来存放最后生成的海报的地址

根据 url 把图片加载到本地方法：

```
export function loadImg (src) {
  console.log('开始下载图片 :', src)
  return new Promise((resolve, reject) => {
    wx.getImageInfo({
      src,
      success (res) {
        console.log('load image url', res)
        resolve(res)
      },
      fail (err) {
        console.log('load image fail:', err)
        reject(new Error(String(err) + '; src:' + src))
      }
    })
  })
}
```

然后把图片画在相应的位置(这里需要注意是不能直接把 canvas 画出来的图展示给用户，因为 canvas 的层级是最高的，如果你有弹框或者页面可以滑动的话，这个体验效果会非常差)：

```
async drawPoster (avatar, qrcode) {
  const ctx = wx.createCanvasContext('share', this)
  const canvasWidth = rpx2px(300 * 2)
  const canvasHeight = rpx2px(400 * 2)

  const posterObj = await loadImg(posterBg)
  ctx.drawImage(posterObj.path, 0, 0, canvasWidth, canvasHeight)
  const qrcodeWidth = rpx2px(153)
  const qrcodeHeight = rpx2px(153)
  const imgObj = await loadImg(qrcode)
  ctx.drawImage(
    imgObj.path,
    rpx2px(400),
    rpx2px(600),
    qrcodeWidth,
    qrcodeHeight
  )
  const avatarWidth = rpx2px(48 * 2)
  const avatarHeight = rpx2px(48 * 2)

  ctx.save()
  ctx.beginPath()
  ctx.arc(rpx2px(46 * 2), rpx2px(355 * 2), rpx2px(24 * 2), 0, 2 * Math.PI)
  ctx.setFillStyle('white')
  ctx.fill()
  ctx.clip()
  const avatarObj = await loadImg(avatar || this.defaultAvar2)
  ctx.drawImage(
    avatarObj.path,
    rpx2px(22 * 2),
    rpx2px(331 * 2),
    avatarWidth,
    avatarHeight
  )
  ctx.restore()
  ctx.draw(false, () => {
    // 把canvas保存成图片文件
    wx.canvasToTempFilePath({
      canvasId: 'share',
      success: (res) => {
        // 把临时的图片文件路径赋给posterImg
        // 然后在页面中动态的去绑定posterImg
        this.posterImg = res.tempFilePath
      },
      fail: () => {}
    }, this)
  })
}
```

方法中用的的 rpx2px 方法是微信小程序的 rpx 转换 px 的方法：

```
export function createRpx2px () {
  const { windowWidth } = wx.getSystemInfoSync()
  return function (rpx) {
    return windowWidth / 750 * rpx
  }
}
```

最最关键的步骤是永远放到最后的  
由于我们平时开发的时候都是用的相对路径，但是相对路径在 mpvue 框架的 js 部分直接写会报错找不到这个文件，所以我们要在最外层使用 const posterBg = require(../../img/\*\*\*.png),然后我们在 js 里面用就可以找到图片了，具体原因我也不知道为啥，如有知道的欢迎指正
