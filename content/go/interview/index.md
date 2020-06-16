---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Interview"
subtitle: "Go语言中文网"
summary: "Go语言面试题"
authors: ["admin"]
tags: ["Go","面试"]
categories: []
date: 2019-12-01T19:15:17+08:00
lastmod: 2019-12-01T19:15:17+08:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "featured"
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []

---

# `Day1`

**题目**：下面这段代码的打印顺序

```go
package main

import (
    "fmt"
)

func main() {
    defer_call()
}

func defer_call() {
    defer func() { fmt.Println("打印前") }()
    defer func() { fmt.Println("打印中") }()
    defer func() { fmt.Println("打印后") }()

    panic("触发异常")
}
```

**答案：**

```go
打印后
打印中
打印前
panic:触发异常
```

**分析**

> `defer` 的执行顺序是先进后出，当初`panic`语句时，会先按照`defer`的后进先出的顺序执行，最后才执行`panic`

