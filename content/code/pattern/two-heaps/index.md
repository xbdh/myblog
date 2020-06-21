---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Pattern-10 Two Heaps"
subtitle: ""
summary: "双堆"
authors: []
tags: ["pattern"]
categories: []
date: 2020-05-23T17:53:19+08:00
lastmod: 2020-05-23T17:53:19+08:00
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

## 1、introduction

给定一组可以划分成两个部分的元素，要求我们求一部分的最小元素，另一部分的最大元素，用two heaps ：Min Heap和Max Heap，可以解决这类的问题，。

## 2、find the median of a number stream

> 设计一个类，能够计算数据流中的中位数，类必须包含两个函数：
>
> insertNum(int num) :存储数据
>
> findMedian() : 返回所有存储进类的数据流的中位数

> 如果数据为奇数个，则中位数为中间两个数之和

![](./2-1.png)