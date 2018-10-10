# HTML5 应用程序缓存

### 应用程序缓存三大优势
1. 离线预览-用户可在应用离线时使用它们
2. 速度-已缓存资源加载得更快
3. 减少服务器负载-浏览器将只从服务器下载更新过或更改过的资源


## Cache Manifest 基础
> 如果启用应用程序缓存，<HTML manifest="demo.appcache"></HTML>

- 每个指定了manifest 的页面在用户对其访问都会被缓存。
- 如果未指定manifest属性，则页面不会被缓存（除非在manifest文件中直接指定了该页面）
- manifest文件的建议的文件扩展名是：".appcache"
- !!!注意: manifest 文件需要正确的 MIME-type, 即"text/cache-manifest"。必须在web服务器进行配置。

## Manifest 文件
> manifest文件是简单的文本文件：它告诉浏览器被缓存内容

```
CACHE MANIFEST
# 2017-12-12 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
login.php

FALLBACK:
/html/ /offline.html

```

#### manifest 文件可分为三部分：
- CACHE MANIFEST - 在此标题下列出的文件将首次下载后进行缓存

```
CHACHE MANIFEST
/theme.css
/logo.gif
/main.js

/*
    上面的 manifest 文件列出了三个资源：
        CSS 文件
        GIF 图像
        JavaScript 文件
        
    当 manifest 文件加载后，浏览器会从网站的根目录下载这三个文件。
    然后，无论用户何时与因特网断开连接，这些资源依然是可用的。
*/

```

- NETWORK - 在此标题下列出的文件需要与服务器的连接，且不会被缓存

```
NETWORK:
login.php

/*
   上面的 NETWORK 小节规定文件 "login.php" 永远不会被缓存，且离线时是不可用的。
   
   可以使用星号来指示所有其他其他资源/文件都需要因特网连接：
   NETWORK:
    *
    
*/
```

- FALLACK - 在此标题下列出的文件规定当页面无法访问时的退会页面（比如404页面）

```
FALLBACK:
/html/ /offline.html

/*
    上面的 FALLBACK 小节规定如果无法建立因特网连接，则用 "offline.html" 替代 /html5/ 目录中的所有文件：
    注意: 第一个 URI 是资源，第二个是替补。
*/
```

## 更新缓存
> 一旦应用被缓存，它就会保存缓存直到发生下列情况：

1. 用户清空浏览器缓存
2. manifest 文件被修改
3. 由程序来更新应用缓存

## 关于应用程序缓存的说明

1. 请留心缓存的内容。
2. 一旦文件被缓存，则浏览器会继续展示已缓存的版本，即使您修改了服务器上的文件。为了确保浏览器更新缓存，您需要更新 manifest 文件。
> 注意: 浏览器对缓存数据的容量限制可能不太一样（某些浏览器设置的限制是每个站点 5MB）。