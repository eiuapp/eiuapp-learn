+++
title = "saleor 设置 templates base"
date = 2019-02-01T00:00:00+08:00
lastmod = 2019-02-01T15:45:36+08:00
tags = ["saleor", "conf"]
categories = ["saleor"]
draft = false
weight = 3001
+++

安装完了 saleor ，下一步，自然就是修改一些我们自己的Logo，页脚之类的东西了。


## 页脚COPYRIGHT {#页脚copyright}

```shell
(saleor) ➜  saleor git:(master) ✗ grep "COPYRIGHT © 2009–2019 MIRUMEE SOFTWARE" -rn ./
./templates/base.html:296:        <div class="col-8 footer__copy-text">COPYRIGHT © 2009–2019 MIRUMEE SOFTWARE</div>
(saleor) ➜  saleor git:(master) ✗
```

把这里修改成我们自己的

```shell
(saleor) ➜  saleor git:(master) ✗ vi templates/base.html
(saleor) ➜  saleor git:(master) ✗ grep "COPYRIGHT © 2009–2019 " -rn ./
../../../templates/base.html:296:        <div class="col-8 footer__copy-text">COPYRIGHT © 2009–2019 谦谦科技</div>
(saleor) ➜  images git:(master) ✗
```


## 页脚Logo {#页脚logo}

只要审查元素就知道，这个 LOGO是 SVG图像。

```html
<div class="col-4">
  <a href="{% url 'home' %}" class="footer__logo float-md-left">
    <svg data-src="{% static "images/logo-document.svg" %}"/>
  </a>
</div>
<div class="col-8 footer__copy-text">COPYRIGHT © 2009–2019 谦谦科技</div>
```

位置在

```shell
(saleor) ➜  saleor git:(master) ✗ ls ./saleor/static/images/logo-document.svg
./saleor/static/images/logo-document.svg
(saleor) ➜  saleor git:(master) ✗
```

所以，我们这里要制造我们自己的SVG图像。

因为我对这个SVG不了解，就直接去<https://icomoon.io/app/#/select>下载了个，并把它命名为 logo-test.svg。并放到指定的位置。

```shell
(saleor) ➜  saleor git:(master) ✗ mv ./logo-test.svg ./saleor/static/images/logo-test.svg
(saleor) ➜  saleor git:(master) ✗ git diff templates/base.html
diff --git a/templates/base.html b/templates/base.html
index f9f67b1..9c5caf0 100644
--- a/templates/base.html
+++ b/templates/base.html
@@ -290,10 +290,10 @@
<div class="row">
<div class="col-4">
<a href="{% url 'home' %}" class="footer__logo float-md-left">
-            <svg data-src="{% static "images/logo-document.svg" %}"/>
+            <svg data-src="{% static "images/logo-test.svg" %}"/>
</a>
</div>
-        <div class="col-8 footer__copy-text">COPYRIGHT © 2009–2019 MIRUMEE SOFTWARE</div>
+        <div class="col-8 footer__copy-text">COPYRIGHT © 2009–2019 千千科技</div>
</div>
</div>
</div>
(END)
```

这样，就完成了页脚logo的替换。


### (可跳过)SVG 生成与展示 {#可跳过--svg-生成与展示}

参考 <https://www.jb51.net/article/105196.htm> 。

```shell
(saleor) ➜  saleor git:(master) ✗ sudo apt update && sudo apt-get install libxml2-dev libxslt1-dev python-dev python-lxml python-cairosvg  -y && pip install pygal lxml cairosvg tinycss cssselect
```


## 页头 logo {#页头-logo}

```
(saleor) ➜  saleor git:(master) ✗ grep "logo-document.svg" -rl ./templates/base.html | xargs sed -i "s/logo-document.svg/logo-document-qq.svg/g"
(saleor) ➜  saleor git:(master) ✗

想了一下，干脆，所有的地方都替换一下。

(saleor) ➜  saleor git:(master) ✗ grep logo-document.svg -rl ./saleor/core/emails.py | xargs sed -i "s/logo-document.svg/logo-document-qq.svg/g"
(saleor) ➜  saleor git:(master) ✗ grep logo-document.svg -rl ./templates/  | xargs sed -i "s/logo-document.svg/logo-document-qq.svg/g"
(saleor) ➜  saleor git:(master) ✗ grep logo-document.svg -rl ./saleor/static/dashboard-next/auth/components/LoginPage/LoginPage.tsx | xargs sed -i "s/logo-document.svg/logo-document-qq.svg/g"
(saleor) ➜  saleor git:(master) ✗
```


## dashboard logo {#dashboard-logo}

```
(saleor) ➜  saleor git:(master) ✗ cp ./logo-document-qq.svg saleor/static/dashboard/images/logo-document-qq.svg
(saleor) ➜  saleor git:(master) ✗ grep "logo.svg" -rl ./templates/dashboard/base.html   | xargs sed -i "s/logo.svg/logo-document-qq.svg/g"
(saleor) ➜  saleor git:(master) ✗
```


## ref {#ref}

-   <https://icomoon.io/app/#/select>
