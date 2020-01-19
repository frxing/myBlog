---
title: 浏览器相关知识
categories: 浏览器 
tags: 浏览器
keywords: [浏览器]
description: 捕捉用户对浏览器的一些操作，包括无记录跳转、点击返回按钮等
---

##### 无历史记录跳转页面

```javascript
window.location.replace(url);    //新文档替换当前文档
```

---

##### 检查用户是否按了回退键

```javascript
window.onbeforeunload = function() {
    return "You work will be lost.";
};
```

---

##### 捕捉浏览器关闭的事件

```javascript
<script language="javascript">
function fnUnloadHandler() {

       alert("Unload event.. Do something to invalidate users session.."); 
}
</script>
<body onbeforeunload="fnUnloadHandler()"></body>
```

---

##### 完全禁止使用后退键

```javascript
<script type="text/javascript">
    window.history.forward();
    function noBack() { window.history.forward(); }
</script>
<body onload="noBack();" onpageshow="if (event.persisted) noBack();" onunload=""></body>
```

---

##### 让返回按钮失效

```javascript
// 在要放回的页面加入下面代码
<script>
    $(document).ready(function(e) {

    if (window.history && window.history.pushState) {
        $(window).on('popstate', function () {
            window.history.pushState('forward', null, '#');
            window.history.forward(1);
        //$("#label").html("第" + (++counter) + "次单击后退按钮。");
        });
    }
    window.history.pushState('forward', null, '#'); //在IE中必须得有这两行
    window.history.forward(1);
    });
</script>
```

---

##### 让浏览器全屏显示网页和取消全屏

```
//点击触发此方法让页面全屏
var launchFullScreen = function (element) {
    if (element.requestFullscreen) {
        element.requestFullscreen();
    } else if (element.msRequestFullscreen) {
        element.msRequestFullscreen();
    } else if (element.mozRequestFullScreen) {
        element.mozRequestFullScreen();
    } else if (element.webkitRequestFullscreen) {
        element.webkitRequestFullscreen(Element.ALLOW_KEYBOARD_INPUT);
    }
},
//点击触发此方法让页面非全屏
cancelFullScreen = function () {
    if (document.exitFullscreen) {
        document.exitFullscreen();
    } else if (document.mozCancelFullScreen) {
        document.mozCancelFullScreen();
    } else if (document.webkitCancelFullScreen) {
        document.webkitCancelFullScreen();
    } else if (document.msExitFullscreen) {
        document.msExitFullscreen();
    } else {
        return true;
    }
};
```

<!--more-->
