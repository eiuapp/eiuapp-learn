使用hugo的hugo-material-docs主题模板来构建书籍

## 下载仓库
```
git clone https://github.com/skyao/learning-istio
```

## 创建文件夹
```
mkdir themes public node_modules _book
```
## 下载themes

这里应当使用skyao的修改后的themes

```
# cd themes && git clone https://github.com/digitalcraftsman/hugo-material-docs
git clone https://github.com/skyao/hugo-material-docs themes/hugo-material-docs
```

## 增加内容

- 删除原content下内容
- 增加 _index.md, index.md 等内容
- 


## .gitignore

## hugo serve

## git 修改

```
rm -rf .git/
git init
git remote add origin XXXXXXXXXXXXX
git add -A
git commit -m "init"
git push origin master
``` 

## .travis.yml

利用Travis CI和Hugo將Blog自動部署到Github Pages

思路
https://zyfdegh.github.io/post/201705-how-i-setup-hugo/
https://blog.yuantops.com/tech/hugo-travis-ci-auto-deploy-to-gh-pages/
https://axdlog.com/zh/2018/using-hugo-and-travis-ci-to-deploy-blog-to-github-pages-automatically/

成功travis.yml文件
https://travis-ci.com/EmielH/tale-hugo/jobs/158730898/config

hugo的重点部分是

```yml
install:
  - wget https://github.com/gohugoio/hugo/releases/download/v0.51/hugo_0.51_Linux-64bit.tar.gz
  - tar -xzvf hugo_0.51_Linux-64bit.tar.gz
  - chmod +x hugo
  - export PATH=$PATH:$PWD
  - hugo version
```

最终 .travis.yml 文件是

```yml
language: go

go:
  - "1.8"  # 指定Golang 1.8

# Specify which branches to build using a safelist
# 分支白名单限制: 只有hugo分支的提交才会触发构建
branches:
  only:
    - master 

install:
# 安装最新的hugo
#  - go get github.com/spf13/hugo 
  - wget https://github.com/gohugoio/hugo/releases/download/v0.51/hugo_0.51_Linux-64bit.tar.gz
  - tar -xzvf hugo_0.51_Linux-64bit.tar.gz
  - chmod +x hugo
  - export PATH=$PATH:$PWD
  - hugo version
  
script:
# 运行hugo命令
  - mkdir -p themes
  - git clone https://github.com/skyao/hugo-material-docs themes/hugo-material-docs
  - hugo

deploy:
  provider: pages # 重要，指定这是一份github pages的部署配置
  skip-cleanup: true # 重要，不能省略
  local-dir: public # 静态站点文件所在目录
  target-branch: gh-pages # 要将静态站点文件发布到哪个分支
  github-token: $GITHUB_TOKEN # 重要，$GITHUB_TOKEN是变量，需要在GitHub上申请、再到配置到Travis
#  fqdn: eiu.app # 如果是自定义域名，此处要填
  keep-history: true # 是否保持target-branch分支的提交记录
  on:
    branch: master # 博客源码的分支
```

### (可跳过).travis.yml文件过程

#### go

```yml
# https://docs.travis-ci.com/user/deployment/pages/
# https://docs.travis-ci.com/user/reference/xenial/
# https://docs.travis-ci.com/user/languages/go/
# https://docs.travis-ci.com/user/customizing-the-build/

dist: xenial
language: go
go:
    - master

# before_install
# install - install any dependencies required
install:
    - go get github.com/gohugoio/hugo    # consume time 70.85s

before_script:
    - rm -rf public 2> /dev/null

# script - run the build script
script:
    - hugo 2> /dev/null
#    - echo "$CNAME_URL" > public/CNAME

deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GITHUB_TOKEN  # Set in travis-ci.org dashboard, marked secure
#  email: $GITHUB_EMAIL
  name: eiuapp # $GITHUB_USERNAME
  verbose: true
  keep-history: true
  local-dir: public
  target_branch: gh-pages  # branch contains blog content
  on:
    branch: master  # branch contains Hugo generator code
```

确实会提示

```
The command "go get github.com/gohugoio/hugo" failed and exited with 1 during .
```

更新成

```
language: go

go:
  - "1.8"  # 指定Golang 1.8

# Specify which branches to build using a safelist
# 分支白名单限制: 只有hugo分支的提交才会触发构建
branches:
  only:
    - hugo 

install:
# 安装最新的hugo
  - go get github.com/spf13/hugo 

script:
# 运行hugo命令
  - hugo

deploy:
  provider: pages # 重要，指定这是一份github pages的部署配置
  skip-cleanup: true # 重要，不能省略
  local-dir: public # 静态站点文件所在目录
  target-branch: gh-pages # 要将静态站点文件发布到哪个分支
  github-token: $GITHUB_TOKEN # 重要，$GITHUB_TOKEN是变量，需要在GitHub上申请、再到配置到Travis
#  fqdn: blog.yuantops.com # 如果是自定义域名，此处要填
  keep-history: true # 是否保持target-branch分支的提交记录
  on:
    branch: master # 博客源码的分支
```

#### python

会出现curl 没有下载成功hugo

#### 

https://discourse.gohugo.io/t/hugo-basic-using-theme-with-hugo-pipes-works-on-local-machine-but-fails-on-travis-ci/15312/3

找到

https://travis-ci.com/EmielH/tale-hugo/jobs/158730898/config

这样是

```yml
install:
  - wget https://github.com/gohugoio/hugo/releases/download/v0.51/hugo_0.51_Linux-64bit.tar.gz
  - tar -xzvf hugo_0.51_Linux-64bit.tar.gz
  - chmod +x hugo
  - export PATH=$PATH:$PWD
  - hugo version
```

### hugo 可执行文件

还有一种方式，是把 hugo 可执行文件，直接放在repo中

https://www.metachris.com/2017/04/continuous-deployment-hugo---travis-ci--github-pages/
https://github.com/Ghoust-game/website/blob/master/.travis.yml


