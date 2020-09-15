---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Leetcode"
subtitle: ""
summary: "算法训练营题目"
authors: []
tags: ["leetcode"]
categories: ["leetcode"]
date: 2020-09-15T21:50:27+08:00
lastmod: 2020-09-15T21:50:27+08:00
featured: false
draft: false
toc: true
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

## 一. 数组、链表、跳表

### 1. 盛最多水的容器

> 给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
>
> 说明：你不能倾斜容器，且 n 的值至少为 2
>

[](https://leetcode-cn.com/problems/container-with-most-water/)

```c++
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

code

```c++
    int maxArea(vector<int>& height) {
        int start=0;
        int end = height.size()-1;
        int maxArea=INT_MIN;

        while(start<end){
            int temp=(end-start)*min(height[end],height[start]);
            maxArea=max(maxArea,temp);
            if(height[start]<height[end]){
                start++;
            }else{
                  end--
            }
        }
        return maxArea;
    }
```

