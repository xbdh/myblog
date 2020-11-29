---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Hugo"
subtitle: ""
summary: "常用操作命令"
authors: []
tags: []
categories: []
date: 2020-05-19T16:50:54+08:00
lastmod: 2020-05-19T16:50:54+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### 

### 1、新建文件

```go
例子1：三个文件在同一文件夹内，index后缀默认路由

// 默认会在content下建文件
hugo new -kind=post post/post-name/index.md
//路由：http://www.example.com/post-name/

hugo new -kind=post post/post-name/a.md
//路由：http://www.example.com/post-name/a/

hugo new -kind=post post/post-name/b.md
//路由：http://www.example.com/post-name/b/


例子2：
hugo new -kind=post post/post-name.md
//路由：http://www.example.com/post-name

-kind 可以省略
```



### 2、启动

```go
hugo server
```



### 3、新建网站

```go
hugo new site [path/name]
```

### 4. 相对路径和绝对路径

```go
如果某文件夹内有：
index.md  1.jpg 2.png

那么 index.md 使用相对路径引用图片 路径为：![](./1.png) ![](./2.png)


如果某文件夹内有：
a.md  1.jpg 2.png
那么 a.md 使用相对路径引用图片 路径为：![](../1.png) ![](../2.png) //上一级目录
这样会导致在tyora中错误显示
```

