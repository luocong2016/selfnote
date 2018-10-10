Cookie
> Cookie 是一些数据, 存储于你电脑上的文本文件中。

当 web 服务器向浏览器发送 web 页面时，在连接关闭后，服务端不会记录用户的信息。

Cookie 的作用就是用于解决 "如何记录客户端的用户信息":
- 当用户访问 web 页面时，他的名字可以记录在 cookie 中。
- 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录。

Cookie 以名/值对形式存储，如下所示:
> username = Lutz

当浏览器从服务器上请求 web 页面时， 属于该页面的 cookie 会被添加到该请求中。服务端通过这种方式来获取用户的信息。

```
// 创建cookie, expires过期时间（UTC 或 GMT）
document.cookie = `username=Lutz;expires= ${(new Date(new Date().getTime()+10 * 1000).toUTCString())}`

// 读取cookie
var str = document.cookie

// 提取（key = value; key2 = value2）
var arr = str.split(';')
var obj = {}
arr.forEach(function(item){
    let tempArr = []
    tempArr = item.split('=')
    obj[tempArr[0].trim()] = tempArr[1].trim()
})

console.log(obj)

```
## JavaScript Cookie 实例

```
/*
    设置cookie值函数
    @param
        cname{string}       键值名
        cvalue{string}      键值
        exdays{number}      过期天数
    @return
        void 0
*/
function setCookie(cname, cvalue, exdays){
    exdays = exdays ? exdays : 1
    var d = new Date()
    d.setTime(d.getTime()+(exdays*24*60*60*1000))
    var expires = "expires="+d.toGMTString()
    document.cookie = cname + "=" + cvalue + "; " + expires
}
setCookie('name', 'Lutz')

/*
    获取cookie值函数
    param
        cname{string}       键值名
    return
        {string}            键值
*/
function getCookie(cname){
    var name = cname + "="
    var ca = document.cookie.split(';')
    
    for(var i=0; i<ca.length; i++) {
        var c = ca[i].trim();
        if (c.indexOf(name)==0){
            return c.substring(name.length, c.length) 
        }
    }
    
    return ""
}
getCookie('name')

/*
    检测cookie 如果不存在,则创建
    @param
        cname{string}       键值名
        cvalue{string}      键值
        exdays{number}      过期天数
    @return
        {string}/{void 0}   存在返回键值，不存在void 0
*/
function checkCookie(cname, cvalue, exdays){
    var key = getCookie(cname)
    
    if(key != ''){
      return key
    }
    
    if(cvalue != ''){
       setCookie(cname, cvalue, exdays) 
    }
    
    return ''
}
```