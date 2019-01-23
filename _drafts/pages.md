---
layout: default
author: lunarwaterfox
title: 使用 GitHub Pages 搭建 Blog
categories: [Github Pages, jekyll]
---

# 关于 GitHub Pages
*[引用：What is GitHub Pages?](https://help.github.com/articles/what-is-github-pages/)*

GitHub Pages 是一种静态站点托管服务，它直接基于 GitHub 的 repository 构建，因此成本低廉。但由于是静态站点，所以依赖于服务端的动态功能都不支持。

# 构建 GitHub Pages 站点
*[引用：Websites for you and your projects.](https://pages.github.com/)*

GitHub Pages 提供两种方式，一种基于用户，一种基于项目。目前只用到了基于用户的方式。

![image](/assets/images/repository.png)

先创建一个名为 username.github.io 的 repository。需要注意的是 Private 项目，仅收费用户可以用作 GitHub Pages。免费用户请选择 Public 。

# Hello World!
提交一个最简单的页面

```console
~ $ git clone https://github.com/username/username.github.io
~ $ cd username.github.io
~ $ echo "Hello World" > index.html
~ $
~ $ git add --all
~ $ git commit -m "Initial commit"
~ $ git push -u origin master
```

# 设置 GitHub Pages

![image](/assets/images/tab.png)

![image](/assets/images/pages.png)

# 查看效果
现在可以访问 username.github.io 来之查看效果了

# 使用 Jekyll
*[引用：Jekyll Homepage](https://jekyllrb.com/)*

Jekyll 是一个将文本转换成静态站点的工具，GitHub Pages 与其深度集成，可以用户很方便生成站点。（当然，如果你不使用 Jekyll ， 也可以自行管理站点文件）

注：GitHub Pages 有自已的一组依赖包，所以建议用 GitHub 的工具创建（而不是官网提供的方法）

# 安装 bundler
*[引用：Jekyll on macOS](https://jekyllrb.com/docs/installation/macos/)*

直接安装 bundler ，会要求 sudo，所以一般安装到用户下
```console
~ $ gem install --user-install bundler
```

然后填加 Path。（注意：Jekyll 官网给的路径不对，用下面的地址）

在 ~/.bash_profile 中填加
```bash
export GEM_HOME=$HOME/.gem/ruby/2.3.0/gems   #替换对应的 ruby 版本号
export PATH=$HOME/.gem/ruby/2.3.0/bin:$PATH  #替换对应的 ruby 版本号
```

# 安装 Github Pages 依赖
*[引用：GitHub Pages Ruby Gem](https://github.com/github/pages-gem)*

建立一个新项目，新建 Gemfile 文件，写入
```
source 'https://rubygems.org'
gem "github-pages", group: :jekyll_plugins
```

执行
```console
~ $ bundle install
```

# 创建一个 Jekyll 项目
新建 _config.yml，index.md 两个文件，加上上面的 Gemfile，现在的目录是这样的

```
Project
├── Gemfile
├── Gemfile.lock
├── _config.yml
└── index.md
```

_config.ym 写入
```
title: The title of your site
description: A short description of your site's purpose
show_downloads: false
google_analytics:
theme: jekyll-theme-cayman
```

index.md 写入
```
---
layout: default
---
```
运行
```console
~ $ bundle exec jekyll build
~ $ bundle exec jekyll serve
```

打开 [localhost:4000](http://localhost:4000) 可以看到效果了

# Jekyll Theme
