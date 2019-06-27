+++
title = "jane主题中安装百度统计"
date = 2018-10-09T10:10:00+08:00
lastmod = 2018-11-29T10:30:19+08:00
tags = ["hugo", "jane", "baidu"]
categories = ["hugo"]
draft = false
weight = 3002
+++

## 获取百度统计代码 {#获取百度统计代码}

登陆百度统计/管理/自有网站/获取代码, 得到形如

```
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "https://hm.baidu.com/hm.js?9dd8aaaaaaaaaaa7a8bde80b13fe8eaf";
  var s = document.getElementsByTagName("script")[0];
  s.parentNode.insertBefore(hm, s);
})();
</script>
```

我们要的就是 "<https://hm.baidu.com/hm.js?9dd8aaaaaaaaaaa7a8bde80b13fe8eaf>" 中的
9dd8aaaaaaaaaaa7a8bde80b13fe8eaf


## 放入config.toml 文件中 {#放入config-dot-toml-文件中}

```
[params]
  baidu_push = false        # baidu push                  # 百度
  baidu_analytics = "9dd8aaaaaaaaaaa7a8bde80b13fe8eaf"      # Baidu Analytics
  baidu_verification = ""   # Baidu Verification
```


## 代码安装检查 {#代码安装检查}

登陆百度统计/管理/自有网站/代码检查, 得到形如：

```
     页面代码安装状态：代码安装正确
```

表示已经加入了百度统计。
