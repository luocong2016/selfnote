```
var date = new Date();
date.toDateString(); // "Mon Oct 08 2018"
date.toGMTString();  // "Mon, 08 Oct 2018 07:34:17 GMT"
date.toISOString(); // "2018-10-08T07:34:17.826Z"
date.toJSON(); // "2018-10-08T07:34:17.826Z"
date.toLocaleDateString(); // "2018/10/8"
date.toLocaleString(); // "2018/10/8 下午3:34:17"
date.toLocaleTimeString(); // "下午3:34:17"
date.toString(); // "Mon Oct 08 2018 15:34:17 GMT+0800 (中国标准时间)"
date.toTimeString() // "15:39:41.279 "15:34:17 GMT+0800 (中国标准时间)"
date.toUTCString(); // "15:40:13.140 "Mon, 08 Oct 2018 07:34:17 GMT"
date.valueOf(); // 1538984057826
+date; // 1538984057826
```