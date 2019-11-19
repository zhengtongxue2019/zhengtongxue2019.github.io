---
title: hexo的基础使用
date: 2019-11-19 11:35:50
tags:
---

## hexo生成的文件夹&文件

* scaffolds 模板文件夹
当新建文章时，hexo会根据scaffolds来建立文件。


*  source 资源文件夹 是存放用户资源的地方。
除_posts文件夹外，以_下划线开头的文件、文件夹和以.点开头的隐藏文件、文件夹将被忽略。
Markdown和HTML文件会被解析并放到public文件夹，其他文件也会被拷贝过去。

* themes 主题文件夹，是存放主题相关的地方

* _config.yml 配置文件，是存放网站相关配置的地方，根据需要进行修改

* package.json 应用程序的信息
主要包含当前应用程序的名字、版本以及依赖集合等信息

* package-lock.json 当前应用程序的安装信息

<!--more-->

## hexo提供的命令
* `hexo init [folder]`初始化一个网站，没有folder时在当前目录进行初始化

* `hexo new [layout] <title>` 使用指定的layout的新建一篇题为title的文章
可选参数
> `hexo new page --path about/me "About me"` 会创建source/about/me.md文件

* `hexo generate` 生成静态文件 可以简写 `hexo g`
可选参数
> -d 文件生成后立即部署
> -w 监视文件的变动
> -f 强制重新生成
> -c 限制最大同时生成的文件数量，默认无限制

* `hexo develop` 发布草稿

* `hexo server` 启动本地服务器，默认4000端口 可以简写 `hexo s`
可选参数
> -p 指定端口号
> -s 只使用静态文件

* `hexo deploy` 部署网站 可以简写 hexo d

* `hexo clean` 清理缓存文件和已生成的静态文件，切换主题后或其他命令不生效时可以使用

## 更换主题
到hexo生成的应用程序目录
使用`git clone xxxTheme.git themes/xxxTheme`下载指定的主题到当前指定的主题目录下
修改_config.xml中的theme为对应的主题名称xxxTheme即可

## hexo的管理
在hexo生成的应用程序文件夹中执行以下命令
1、git init 将其加入版本管理计划中
2、git checkout -b develop 新建并切换到develop分支
3、hexo new xxx 在develop分支中新建xxx文件
4、hexo g 在develop分支中生成
5、hexo s 在develop分支中启动本地local服务器
6、hexo clean 在develop分支中清理
以下操作在github上执行

## 写文章时的注意事项
标签tags 支持多标签，如[tag1, tag2, tag3]，需要注意标签后面的，后面有空格进行分割。

7、在github上新建一个同用户名相同的项目，不要初始化任何东西。
