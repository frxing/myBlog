---
title: 原生js实现jsonp封装
categories: jsonp 
tags: jsonp
keywords: [jsonp]
description: 实现jsonp方法封装，可以像使用ajax一样使用jsonp
---
JSONP即JSON with Padding。由于同源策略的限制，XmlHttpRequest只允许请求当前源（域名、协议、端口）的资源。如果要进行跨域请求， 我们可以通过使用html的script标记来进行跨域请求，并在响应中返回要执行的script代码，其中可以直接使用JSON传递javascript对象。 这种跨域的通讯方式称为JSONP。

我们可以动态的去创建一个script标签，利用他的src属性没有跨域的限制来实现的，相当于我们引入一个js文件

附上源码：
```
jsonp: function (json){
　　json = json || {};
　　if(!json.url)return;
　　json.cbName = json.cbName||'cb';
　　json.data = json.data||{};

　　json.data[json.cbName] = 'show'+Math.random();
　　json.data[json.cbName] = json.data[json.cbName].replace('.','');

　　var arr = [];
　　for(var i in json.data){
　　　　arr.push(i+'='+encodeURIComponent(json.data[i]));
　　}
　　var str = arr.join('&');

　　window[json.data[json.cbName]]=function(result){
　　　　json.success&&json.success(result);
　　　　oH.removeChild(oS);
　　　　window[json.data[json.cbName]]=null;
　　};
　　var oH = document.getElementsByTagName('head')[0];
　　var oS = document.createElement('script');
　　oS.src=json.url+'?'+str;
　　oH.appendChild(oS);
　　oS.onerror = function(){
　　　　window[json.data[json.cbName]]=null;
　　　　oH.removeChild(oS);
　　　　json.error&&json.error();
　　}
}
```
具体用法：
```
jsonp({
　　url: '----',
　　success: function (json) {

　　.................
　　},
　　error: function(){
　　..................
　　}
});
```
一定要注意如果我们后端返回的是json对象的话，是无法跨域返给我们的，我们的请求的时候给后端传了一个用于回调function(){}，后端必须把要返回的json对象放到这个function(){}里面，然后把这个方法返回给我们，我们才可以解析适用

java返回示例：
```
return request.getParameter("jsonpcallback")+"("+s+")";  
```