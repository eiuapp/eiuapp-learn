---
title: "Gem Install Faq"
date: 2018-02-26T09:23:11+08:00
draft: false
tags: ["ruby", "gem", "faq"]
categories: ["ruby"]
subtitle: ""
descriptions: ""
bigimg:
---

#### faq, ruby.h not found

```
ubuntu@VM-0-12-ubuntu:~/registry/tomtsang-rootsongjc-cheatsheet$ bundle install
...
...
To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /tmp/bundler20180226-1352-qwjujmnokogiri-1.8.0/extensions/x86_64-linux/2.3.0/nokogiri-1.8.0/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /tmp/bundler20180226-1352-qwjujmnokogiri-1.8.0/gems/nokogiri-1.8.0 for inspection.
Results logged to /tmp/bundler20180226-1352-qwjujmnokogiri-1.8.0/extensions/x86_64-linux/2.3.0/nokogiri-1.8.0/gem_make.out

An error occurred while installing nokogiri (1.8.0), and Bundler cannot continue.
Make sure that `gem install nokogiri -v '1.8.0'` succeeds before bundling.

In Gemfile:
  github-pages was resolved to 156, which depends on
    jekyll-mentions was resolved to 1.2.0, which depends on
      html-pipeline was resolved to 2.7.0, which depends on
        nokogiri
ubuntu@VM-0-12-ubuntu:~/registry/tomtsang-rootsongjc-cheatsheet$
```

这个时候, 依据提示, 要去安装 `ffi`, 好, 来吧.

```
sudo gem install ffi -v '1.9.18'
```

如果说报出, 没有找到 `ruby.h` 文件的错误,则说明没有安装 ruby-dev 包.那么去安装吧.
```
sudo apt install ruby-dev
```

#### faq, 


```
ubuntu@VM-0-12-ubuntu:~/registry/tomtsang-rootsongjc-cheatsheet$ bundle install
...
...
To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /tmp/bundler20180226-1352-qwjujmnokogiri-1.8.0/extensions/x86_64-linux/2.3.0/nokogiri-1.8.0/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /tmp/bundler20180226-1352-qwjujmnokogiri-1.8.0/gems/nokogiri-1.8.0 for inspection.
Results logged to /tmp/bundler20180226-1352-qwjujmnokogiri-1.8.0/extensions/x86_64-linux/2.3.0/nokogiri-1.8.0/gem_make.out

An error occurred while installing nokogiri (1.8.0), and Bundler cannot continue.
Make sure that `gem install nokogiri -v '1.8.0'` succeeds before bundling.

In Gemfile:
  github-pages was resolved to 156, which depends on
    jekyll-mentions was resolved to 1.2.0, which depends on
      html-pipeline was resolved to 2.7.0, which depends on
        nokogiri
ubuntu@VM-0-12-ubuntu:~/registry/tomtsang-rootsongjc-cheatsheet$
```

好, 那就安装 nokogiri

```
ubuntu@VM-0-12-ubuntu:~/registry/tomtsang-rootsongjc-cheatsheet$ sudo gem install nokogiri -v '1.8.0'
Building native extensions.  This could take a while...
ERROR:  Error installing nokogiri:
	ERROR: Failed to build gem native extension.

    current directory: /var/lib/gems/2.3.0/gems/nokogiri-1.8.0/ext/nokogiri
/usr/bin/ruby2.3 -r ./siteconf20180226-2229-iju4w6.rb extconf.rb
checking if the C compiler accepts ... yes
Building nokogiri using packaged libraries.
Using mini_portile version 2.2.0
checking for gzdopen() in -lz... no
zlib is missing; necessary for building libxml2     ##  这里是一个关键点了, zlib 没安装
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--without-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/usr/bin/$(RUBY_BASE_NAME)2.3
	--help
	--clean
	--use-system-libraries
	--enable-static
	--disable-static
	--with-zlib-dir
	--without-zlib-dir
	--with-zlib-include
	--without-zlib-include=${zlib-dir}/include
	--with-zlib-lib
	--without-zlib-lib=${zlib-dir}/lib
	--enable-cross-build
	--disable-cross-build

To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /var/lib/gems/2.3.0/extensions/x86_64-linux/2.3.0/nokogiri-1.8.0/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /var/lib/gems/2.3.0/gems/nokogiri-1.8.0 for inspection.
Results logged to /var/lib/gems/2.3.0/extensions/x86_64-linux/2.3.0/nokogiri-1.8.0/gem_make.out
ubuntu@VM-0-12-ubuntu:~/registry/tomtsang-rootsongjc-cheatsheet$
```

看上面这里说了 `zlib is missing`, 所以要安装zlib

```
sudo apt install zlib1g
sudo apt install zlib1g-dev
```