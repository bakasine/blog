---
title: (已失效)如何申请msn.com、live.com、live.cn等后缀邮箱
date: 2018-06-27 02:20:23
cover: /images/preview12.jpg
coverWidth: 1600
coverHeight: 900
tags: 
    - email
categories: 
    - others
---

> 登录邮箱

``` python
https://account.live.com/AddAssocId
```

> F12(打开调试)  -> Console(控制台) ->  粘贴脚本 （回车）

``` bash
var sub = window.prompt('1. 输入域名，支持live.com/msn.com/live.cn等等...\r\n 2. 更多后缀请访问：https://51.ruyo.net/p/3194.html\r\n 提醒：直接点击取消即可前往上面的网址！\r\n 3. 点击确定后，页面会刷新。\r\n 4. 页面刷新后，请添加你想要的别名即可。','live.com');if(sub){document.getElementById("SingleDomain").value = sub;document.getElementById("idSingleDomain").innerText = "@"+sub;document.getElementById("AssociatedIdLive").value="a";document.getElementById("SubmitYes").click();}else{window.open("https://51.ruyo.net/p/3194.html")};
```

> 转载自

# [如有乐享](https://51.ruyo.net/1327.html)
