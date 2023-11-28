---
title: "使用hugo在github上部署个人博客的详细教程"
date: 2023-11-28T10:23:48+08:00
draft: false
weight: 1
---

## 使用hugo在github上部署个人博客的详细教程

一、github端的操作

​		1.在github网站的全局设置中设置一个token。

​		2.创建一个名称为blog的仓库用来存放hugo博客的源码。

​		3.设置仓库的secrets中actions的value为全局token，并将这个secret的key名称和hugo中.yml文件中的配置对应,此处配置为PERSONAL_TOKEN。

二、本地配置操作

​		1.初始化一个hugo网站

```
hugo new site blog
```

​		2.git初始化   加载主题  配置主题  配置baseurl

```
git init
git submodule add https://github.com/yzccjg/dark-theme-editor.git themes/dark-theme-editor
```

```
在hugo.toml文件中配置
theme = 'dark-theme-editor'
```

```
在hugo.toml文件中配置
baseURL = 'https://yzccjg.github.io/blog/'
```

​	

​		3.配置.yml文件，在根目录下新建.github>workflows>hugo-deploy.yml

```
name: deploy

on:
    push:
    workflow_dispatch:

jobs:
    build:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@v2
              with:
                  submodules: true
                  fetch-depth: 0

            - name: Setup Hugo
              uses: peaceiris/actions-hugo@v2
              with:
                  hugo-version: "latest"

            - name: Build Web
              run: hugo

            - name: Deploy Web
              uses: peaceiris/actions-gh-pages@v3
              with:
                  PERSONAL_TOKEN: ${{ secrets.PERSONAL_TOKEN }}
                  EXTERNAL_REPOSITORY: yzccjg/blog
                  PUBLISH_BRANCH: gh-pages
                  PUBLISH_DIR: ./public
                  commit_message: ${{ github.event.head_commit.message }}
```

​		4.新建一篇文章并随意输入内容后测试

```
hugo new test.md
记得将draft:true改为false
```

```
hugo server
```

​		5.git推送忽略public配置

```
在根目录下新建文件名为.gitignore文件
其中输入public/
```



三、将本地和远端进行关联

​		1.准备工作

```

查看和清除旧的配置
git config --global -l  #查看全局配置
git config --global --edit  #删除全局配置，这个得会vim、

下面两条是笨方法进行配置
git config --global --unset user.name
git config --global --unset user.email

配置全局用户和邮箱，这个如果没有配置，在push代码时会自动提示
git config --global  user.name "yzccjg"
git config --global  user.email "yuzecheng520@qq.com"

```

​		2.和远程仓库进行关联

```
git remote remove origin  #删除远程仓库
git remote add origin https://github.com/yzccjg/blog.git
git remote set-url origin https://你的token@github.com/yzccjg/blog ##设置远程仓库origin的url
```

​		3.准备提交

```
git add .
git commit -m 'msg'
git branch -M main
```

​		4.提交代码

```
git push -u origin main
```







