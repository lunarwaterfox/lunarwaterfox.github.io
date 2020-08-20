---
layout: default
author: lunarwaterfox
title: 使用 org 书写用于 jekyll 的 markdown
categories: [org, jekyll, emacs]
---


# 目录 - TableContent

org export 出的 markdown 文件会自动填加一个目录(TableContent)，jekyll 不需要

在文件头写入

```org
#+OPTIONS: toc:nil
```

关闭 TableContent 导出


# 标题 - headings

org 的 headings, 快捷键: M-enter

```org
一级标题
二级标题
```

对应 markdown

```markdown
# 一级标题
## 二级标题
```


# 无序列表 - unnumbered headings

org 和 markdown 同样都是

```org
- item1
- item2
```


# 引用块 - quote block

org 的引用块, 快捷键: <q TAB

```org
#+BEGIN_QUOTE
quote content
#+END_QUOTE
```

对应 markdown

```markdown
> quote content
```


# 超链接

org 的超链接, 快捷键: C-c C-l

```org
[[http://address][链接描述]]
```

对应 markdown

```markdown
[链接描述](http://address)
```


# 引用图片

org 的图片引用与普通超链接一样, 快捷键: C-c C-l

```org
[[http://address]]
```

对应 markdown

```markdown
![链接描述](http://address)
```


# 特殊字符转义

常用字符转义表

例如: GIT_SSH <= GIT\under{}SSH

其他转义规则可查询 M-x org-entities-help