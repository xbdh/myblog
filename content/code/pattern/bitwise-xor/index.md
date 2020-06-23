---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Pattern-13 Bitwise Xor"
subtitle: ""
summary: "位运算"
authors: []
tags: ["pattern"]
categories: []
date: 2020-05-23T17:55:41+08:00
lastmod: 2020-05-23T17:55:41+08:00
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

异或操作

## 2、missing number and single number

>

code:

```c++
int findMissingNumber(const vector<int> &arr) {
    int n = arr.size() + 1;
    int s1 = 1;
    for (int i = 2; i <= n; i++) {
        s1 ^= i;
    }

    int s2 = arr[0];
    for (int j = 1; j < n - 1; j++) {
        s2 ^= arr[j];
    }

    return s1 ^ s2;
}
```

