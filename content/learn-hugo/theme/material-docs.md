---
date: 2018-09-09T21:07:13+01:00
title: material-docs主题
menu:
  main:
    parent: "theme"
weight: 320
keywords:
- theme
- material-docs
- material-docs主题
- hugo-material-docs
description : "介绍hugo的material-docs主题，使用方式和订制化"
---

## 主题介绍

material-docs官网：

http://themes.gohugo.io/theme/material-docs/

这个主题非常适合用于文档，左边的导航栏支持多级菜单，样式也简单美观，而且对移动设备支持很好。

我主要用它来做学习笔记，替代之前长期使用的gitbook（gitbook的问题主要是本地生成内容速度太慢，而gitbook官方网站访问速度又不好）。当前这个hugo学习笔记用的就是此主题。

## 准备工作

### Fork并定制

material-docs的github仓库已经有很长时间没有更新，基本停留在两年前。从实践中看，有很多问题，对新的hugo版本的支持也很不好。

因此，采用fork的方式，建立自己的仓库：

https://github.com/skyao/hugo-material-docs

期间做了很多了更新：

1. PR那边有很多已经被发现而且修订提交的问题，但是一直没有merge，我手工合并到自己的仓库
2. 弃用google统计，改用百度统计，具体做法和 [academic](../academic/analytices/) 中的做法是一样的
3. 同样进行了本地加速，参照  [academic](../academic/local-speed-up/) 中的做法
4. 修改了layout文件，主要是删除了版权申明，作者信息，下载修改为意见反馈，现在的版面非常的干净，没有任何多余的东西
5. 汉化，没有做标准的i18n，因为是给自己定制，因此直接在layout文件中修改为中文。
6. 修改了exampleSite的内容，删除原有的页面和图片，改为学习笔记的常见页面如介绍，安装等，方便后续使用
7. 提供了简单的gitbook模版文件支持，可以生成一个简单带有文字说明和链接的页面，以便将原有gitbook用户导流到新的hugo笔记

## 使用方式

以新建一个学习笔记为例，详细描述需要的步骤和命令。

### 新建学习笔记

1. 在github中新建仓库，名为 **learning-cilium**的Cilium学习笔记

2. 在本地新建一个hugo站点

    ```bash
    hugo new site learning-cilium
    cd learning-cilium
    ```

3. 下载更新主题的脚本并执行

    ```bash
    wget https://raw.githubusercontent.com/skyao/hugo-material-docs/master/exampleSite/update_theme.sh
    chmod +x update_theme.sh
    ./update_theme.sh
    ```

	命令完成之后，`themes/hugo-material-docs`目录下就有我们自己的主题文件内容。

4. 从`exampleSite`目录复制需要的站点初始化文件到新站点

    ```bash
    cd themes/hugo-material-docs
    # 复制config.toml文件
    cp config.toml ../../../config.toml
    # 复制content目录和static目录
    cp -r content/ ../../../content/
    cp -r static/ ../../../static/
    # 复制gitignore文件
    cp gitignore-example ../../../.gitignore
    ```

5. 修改新站点的`config.toml`文件

    以下内容必须修改：

    ```toml
    title = "学习笔记"
    repo_url = "https://github.com/skyao/learning-hugo"
    ```

6. 此时已经开始运行`hogo server`命令来查看新站点的内容了

7. 继续修改站点内容

	主要是content目录下的`index.md`以及`installation`和`introduction`两个目录下的文件，也可以继续修改config.toml文件，可以从浏览器上实时看到修改后的效果

8. 复制`content/index.md`文件到根目录下，作为README.md文件

    ```bash
    cp content/index.md README.md
    ```

    然后删除文件头部的hugo元数据。

9. 提交第一个初始版本到github

    ```bash
    git init
    # 检查一下要提交的文件是否如预期
    git status
    git add -A
    git commit -m "first version"
    git remote add origin git@github.com:skyao/learning-cilium.git
    git push -u origin master
    ```
10. 和gitbook共存
	
	由于原来的读书笔记是gitbook格式，因此为了让原来gitbook那边的老用户有机会找回到新的基于hugo的笔记，需要在gitbook那边好歹留一个简单页面说明。

	为此，保留gitbook的最少文件，如`book.json`和`README.md`即可。是的，SUMMARY.md文件都是可以不用的，而且README.md文件本来也可以用来作为在github仓库的首页文档，因此实质上只是多了一个`book.json`文件。

	内容示意如下：
	
	```json
	{
	  "gitbook": ">=3.2.0",
	  "description": "Hugo学习笔记",
	  "language": "zh-hans",
	  "author": "敖小剑",
	  "extension": null,
	  "generator": "site",
	  "links" : {
	    "sidebar" : {
	        "敖小剑的博客": "https://skyao.io"
	    }
	  },
	  "structure" : {
	    },
	  "plugins": [
	    "-search",
	    "-sharing"
	  ],
	  "pluginsConfig": {
	  },
	  "title": "Hugo学习笔记",
	  "variables": {
	  }
	}
	```

11. 最后，准备好`.gitignore`文件

	```bash
	.*
	!.gitignore
	_book/
	node_modules/
	public/
	themes/
	update_theme.sh
	```

## 扩展和增强

### 三次菜单和自动隐藏

默认，material-docs主题的主菜单，只显示两层。修改代码之后可以支持三层菜单显示，以应对笔记内容比较多的情况。

但是也引发了另外一个问题，内容比较多的笔记，三级菜单一起展开，主菜单项的内容实在太多了。而且material-docs主题的主菜单是每个页面都嵌入的，不是iframe那种，导致菜单项比很多页面的实际内容都要多。因此考虑实现菜单项的自动隐藏，当菜单项（或者子项）没有被打开时，就只打开一级菜单。

需要修改`hugo-material-docs/layouts/partials/nav.html`文件：

```html
{{ $currentNode := . }}

{{ range .Site.Menus.main.ByWeight }}
{{ $.Scratch.Set "currentMenuEntry" . }}
<li>
  {{ if .HasChildren }}
    {{ partial "nav_link" $currentNode }}
{{ if hasPrefix ( $.Permalink | relURL | printf "%s" )  .URL }}
    <ul>
      {{ range .Children }}
        {{ $.Scratch.Set "currentMenuEntry" . }}
        <li>
          {{ if .HasChildren }}
            {{ partial "nav_link" $currentNode }}
{{ if hasPrefix ( $.Permalink | relURL | printf "%s" )  .URL }}
            <ul>
              {{ range .Children }}
                {{ $.Scratch.Set "currentMenuEntry" . }}
                {{ partial "nav_link" $currentNode }}
              {{ end }}
            </ul>
{{ end }}
          {{ else }}
            {{ partial "nav_link" $currentNode }}
          {{ end }}
        </li>
      {{ end }}
    </ul>
{{ end }}
  {{ else }}
    {{ partial "nav_link" $currentNode }}
  {{ end }}
</li>
{{ end }}
```

因为不熟悉代码，暂时通过写笨代码的方式实现了第三级菜单，最好是能用递归的方式，这样就可以实现无限级的菜单了。`{{ if hasPrefix ( $.Permalink | relURL | printf "%s" )  .URL }}` 用来判断是当前菜单或者当前菜单的子项是否打开，这个判断在第二级和第三级菜单中执行。

## 发现并修复的问题

### 主菜单上当前页的H2菜单不显示

这个主题有个特性，可以在主菜单中，在当前页面下会自动增加所有H2 titile作为子菜单。这个特性相当于自动增加了一级菜单，对于内容长的页面很有用。

发现，如果baseURL是完全域名如"https://skyao.io/"或者"http://localhost:1313"时，工作正常。但是，如果baseURL是"https://skyao.io/learning-hugo/"或者"http://localhost:1313/learning-hugo/"这样带有子目录时，就会失效。

检查发现，是`themes/hugo-material-docs/layouts/partials`下的`nav_link.html`中，这段代码判断错误：

```html
{{ $currentMenuEntry := .Scratch.Get "currentMenuEntry" }}
{{ $isCurrent := eq .Permalink ($currentMenuEntry.URL | absURL | printf "%s") }}
```

这里判断菜单项是否是当前页面时，判断的方式是将当前页面的实际URL Permalink(内容如`https://skyao.io/learning-hugo/installation/seo/`) 和菜单项URL地址`$currentMenuEntry.URL`(内容如`/installation/seo/`)进行比较，如果一致则表示是当前页面。为了得到菜单项URL地址的绝对路径，需要使用absURL函数，如`($currentMenuEntry.URL | absURL | printf "%s")`。

但是这里的absURL函数输出结果居然是不正确的！baseURL为`https://skyao.io/learning-hugo/` ，参数为 `/installation/seo/`时，函数absURL的结果居然是`https://skyao.io/installation/seo/`，`learning-hugo/`子目录消失了。

修改方式为通过获取Permalink地址的相对路径，然后和`$currentMenuEntry.URL`比较，这样就避开了absURL函数的问题：

```html
{{ $currentMenuEntry := .Scratch.Get "currentMenuEntry" }}
{{ $isCurrent := eq ( .Permalink | relURL | printf "%s" ) $currentMenuEntry.URL }}
```

这里还发现一个要注意的地方，在config.toml文件中，主菜单项的配置，url一定要用"/"开头，即：

```toml
[[menu.main]]
	name   = "安装"
	identifier = "installation"
	url    = "/installation/"		# 这里一定不要设置为"installation/"
	weight = 200
```

测试验证OK！

### Index页面变成空白页

这个问题来历有些特别，material-docs主题因为太长时间没有维护，所以和新版本的hugo有非常大的差异。

体现在`_index.md`文件的使用上，material-docs主题的exampleSite，跑在新版本的hugo上，各个页面都直接报错"404"！看public目录发现，content目录下的除index.md文件外，其他内容包括子目录都没有被处理。

解决的方式就是在每个目录（包括根目录）下放置一个`_index.md`文件。

但是，首页会偶尔出现问题，`/`这个页面偶尔会出现空白，没有内容，有时有能正常显示`index.md`文件的内容。没有规律，有时发生。

最后，解决的方式是：

1. 修改主题布局文件 `hugo-material-docs/layouts/index.html`

	```html
	{{ range where .Site.Pages "Type" "index" }}
	<h1>{{ .Title }} {{ if .IsDraft }} (Draft){{ end }}</h1>

	{{ .Content }}
	{{ end }}
	```

	删除这里的range循环，不要这个功能（可以自动从其他type为index的页面提取Content然后聚合在最后生成的index.html文件里面，不好用）：

	```html
	<h1>{{ .Title }} {{ if .IsDraft }} (Draft){{ end }}</h1>

	{{ .Content }}
	```

	​

2. 在根目录下只放置一个`_index.md`文件，原来`index.md`的内容都放在这个文件里面。或者说，将`index.md`文件改名为"_index.md"。其他目录下还是同时放`index.md`和`_index.md`文件。

### 上一页和下一页的准确性

页面排序有两个位置：

1. 左边的主菜单栏：这个是通过`config.toml`文件中定义的main menu的weight来决定，配合每个页面的weight。

	注意这时单个目录下的子页面的排序是在当前目录下进行。

2. 每个页面的底部有上一页和下一页

	默认是按照时间（即每个页面上的data信息）排序，所以和前面用weight排序的结果就会很有大出入。

	因此，需要修改模板文件将排序方式修改为按照weight排序。

	其次，这里的排序是全局（即所有页面）进行排序。和前面每个目录下单独排序不一样，因此要让页面的上一页和下一页显示的内容和主菜单一致，就必须保证两个方法排序的一致性。

	推荐的方式是：子目录之间分配好weight的范围，比如a目录是100-199，b目录是200-299。然后a目录下的页面的weight就一定要取值在100-199中，b目录下的页面的weight就一定要取值在200-299。

## 待解决的问题

### 主题已经不再更新

作者已经消失，一年多没有更新，包括PR也没有人合并。

随着hugo的版本越来越新，这个主题的问题也越来越多。希望后面有人继续维护这个主题，或者有类似的替代gitbook适合编写学习笔记类的主题出现。