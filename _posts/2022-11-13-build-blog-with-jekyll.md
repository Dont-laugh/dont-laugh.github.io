---
layout: post
title: "使用 Jekyll 构建博客平台"
author: "Dont Laugh"
categories: technique
tags: [blog,website]
image: title/Lagrange.png
---

> 目标：从零构建个人博客平台
>
> 关键词：GitHub Pages, Jekyll, Lagrange

# GitHub Pages

GitHub Pages[^1] 是 GitHub 推出的免费自动化构建网站的服务，你只需要按照规则设定好你的仓库里提供 markdown 文件，GitHub 后台就会自动根据你的内容生成静态网页，这是我们今天要构建的博客平台的舞台。

首先你需要有一个特殊命名的仓库，规则为：`username.github.io`。其中 `username` 可以是任意合法字符，一般写你的 GitHub 用户名。

⚠️ **注意：该仓库必须是 Public。**

![new-repository](../assets/img/content/2022-11-13.png)

仓库建好后，你可以新建一个 `index.html` 文件，上传后 GitHub 会自动构建网站。第一次构建将花费 5-10 分钟，然后你可以打开网站（https://username.github.io）看看效果。

我的博客网站就是通过 GitHub Pages 自动生成的。



# Jekyll

Jekyll[^2] 是一个简单的、博客感知的静态网站生成器，它基于模板，把你的 md 文件制作成完整的网站。这里有一篇文章能帮助你理解 Jekyll 和 GitHub Pages 之间的关系：[A Guide to Creating and Hosting a Personal Website on GitHub](https://jmcglone.com/guides/github-pages/)

GitHub Pages 的后台就是通过 Jekyll 来生成静态网页的，作为程序员，我们在正式写博客之前有必要对 Jekyll 有个基础的了解。

## Jekyll 目录结构

先来看看 Jekyll 的目录结构[^3]，混个脸熟，后面还会遇到。

```
.
├── _config.yml                          # Jekyll配置文件
├── _drafts                              # 存放草稿的文件夹
|   └── on-simplicity-in-technology.md   # 草稿不需要加日期
├── _includes                            # 可以加载这些包含部分，方便重用
|   ├── footer.html
|   └── header.html
├── _layouts                             # 布局文件夹，用来定义各界面的布局
|   ├── default.html
|   └── post.html
├── _posts                               # 正式版博客的文件
|   └── 2009-04-26-your-first-blog.md    # 正式版博客需要加日期
├── _site                                # build之后的网站文件都在这儿
└── index.html                           # 默认打开的网页定义
```

## 头信息 (Front Matter)

头信息是 Jekyll 中的核心 feature，任何包含了 YAML 样式的头信息的文件，都会被当做特殊文件处理。头信息必须在文件开头，并且 YAML 样式部分必须在两行三虚线 `---` 之间。例子：

```yaml
---
layout: post
title: Blogging Like a Hacker
---
```

在头信息里，你可以设置一些预定义好的变量，来控制博客的表现。

| 变量名称   | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| layout     | 指定使用哪个模板，不带后缀，模板文件必须定义在 `_layouts` 文件夹下 |
| title      | 文章的标题                                                   |
| author     | 文章的作者                                                   |
| categories | 文章的分类，多个字符串之间用逗号分隔                         |
| tags       | 文章的标签，多个字符串之间用逗号分隔                         |
| date       | 默认采用文章文件名字中的日期，设置后会覆盖默认值；格式`YYYY-MM-DD HH:MM:SS +/-TTTT`，时，分，秒和时区都是可选的 |
| published  | 若设置为 false，该文章不会出现在生成后的网站中               |

## 博客规则

「专注博客本身」，这是 Jekyll 的亮点特性，但同时意味着你要遵循 Jekyll 制定的规则。

Jekyll 要求所有博文必须在 `_posts_` 文件夹下，支持 md 和 textile 两种格式，且文件名称要按照以下格式：

```
年-月-日-标题.MARKUP
```

其中 `年`是 4 位数字，`月`和`日`都是 2 位数字，`MARKUP`代表扩展名。

## 本地生成

如果你不想上传后等待 GitHub 构建，马上看到生成的网页是什么样子的，或者在上传正式博客之前预览一遍，你就要配置 Jekyll 的开发环境了。

在 Mac 上安装 Jekyll 很简单，只需要一行命令：

```shell
gem install jekyll
```

在 Windows 上安装则要麻烦一些，你需要先安装软件包管理器 Chocolatey[^4]，然后安装 Ruby 和 github-pages，具体过程参照：[How to install Jekyll and pages-gem on Windows (x64)](https://jwillmer.de/blog/tutorial/how-to-install-jekyll-and-pages-gem-on-windows-10-x46)

安装好之后，通过以下命令构建网站并启动本地服务器：

```shell
jekyll serve -w
```

其中，`-w` 是 `--watch` 的缩写，表示有文件变动时自动重新构建，默认地址是：http://127.0.0.1:4000。

如果你想预览草稿（存放在 `_drafts` 文件夹下的文件），加上 `--drafts` 参数即可，Jekyll 会把草稿的日期自动填补成当天。

```shell
jekyll serve --drafts -w
```



# Lagrange

Jekyll 只提供最基础的自动化构建网站功能，想要好看、简洁的界面，还需要寻找合适的主题。这里有一些网站可供查找你想要的主题：

[Supported themes - GitHub Pages](https://pages.github.com/themes/)

[Jekyll Themes](http://jekyllthemes.org/)

[Free Jekyll Themes](https://jekyllthemes.io/free)

你也可以直接在 GitHub 直接搜索 `Jekyll Theme`，能找到一大堆优秀的开源主题。

我的博客采用的主题是 Lagrange[^5]，我的博客 GitHub 仓库地址：https://github.com/Dont-laugh/dont-laugh.github.io。

Lagrange 的目录结构如下，基本上只是对 Jekyll 官方的扩展。

```
.
├── _data                 # Data files
|  └── settings.yml       # Theme settings and custom text
├── _includes             # Theme includes
├── _layouts              # Theme layouts (see below for details)
├── _posts                # Where all your posts will go
├── assets                # Style sheets and images are found here
|  ├── css                # Style sheets go here
|  |  └── main.css        # Main CSS file
|  |  └── syntax.css      # Style sheet for code syntax highlighting
|  └── img                # Images go here
├── menu                  # Menu pages
├── _config.yml           # Site build settings
├── Gemfile               # Ruby Gemfile for managing Jekyll plugins
├── index.md              # Home page
├── LICENSE.md            # License for this theme
├── README.md             # Includes all of the documentation for this theme
└── rss-feed.xml          # Generates RSS 2.0 file which Jekyll points to
```

如果你也想使用 Lagrange 主题，你可以下载整个 Lagrange 仓库，然后清空 `_posts` 文件夹。根据需要修改 `_data/settings.yml` 中的 menu 列表（定义了博客标题下面一行的菜单栏），以及 `menu` 文件夹下的 md 文件（每个菜单对应的网页）。



# 经验总结

- Q1：如何使用图片，让本地预览和正式构建版本都能正确查看？

把图片放到 `./assets/img` 文件夹下，然后把 `_config.yml` 中的 permalink 从带子目录的形式改成不带子目录的形式，例如（permalink: /blog/:year-:month-:day-:title），这样生成后的博客就不会被自动分配子目录了。

```
# _config.yml
# permalink:  /blog/:year/:month/:day/:title ×
permalink:    /blog/:year-:month-:day-:title

# 目录结构：
.
├── _drafts
├── _posts
|   ├── 2022-11-13-test-blog.md
├── assets
|   ├── img

# 图片URL
![test-picture](../assets/img/test.png)
```



**参考文献：**

[^1]:https://pages.github.com
[^2]:英文：https://jekyllrb.com，中文：https://jekyllcn.com
[^3]: http://jekyllcn.com/docs/structure
[^4]:https://chocolatey.org/install
[^5]:https://github.com/LeNPaul/Lagrange

