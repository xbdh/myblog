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

### 11. 盛最多水的容器

> 给你 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。
>
> 说明：你不能倾斜容器，且 n 的值至少为 2
>

https://leetcode-cn.com/problems/container-with-most-water/

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49
```

code

```c++
int maxArea(vector<int> &height) {
    int start = 0;
    int end = height.size() - 1;
    int maxArea = INT_MIN;

    while (start < end) {
        int temp = (end - start) * min(height[end], height[start]);
        maxArea = max(maxArea, temp);
        if (height[start] < height[end]) {
            start++;
        } else {
            end--
        }
    }
    return maxArea;
}
```



### 283. 移动零

https://leetcode-cn.com/problems/move-zeroes/

> 给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

example

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

code

```c++
void moveZeroes(vector<int> &nums) {
    // 最后的非零值的索引
    int lastNonZeroFoundAt = 0;
    for (int  cur = 0; cur < nums.size(); cur++) {
        if (nums[cur] != 0) {
            swap(nums[lastNonZeroFoundAt++], nums[cur]);
        }
    }
}

```

### 70. 爬楼梯

https://leetcode-cn.com/problems/climbing-stairs/

> 假设你正在爬楼梯。需要 n 阶你才能到达楼顶。
>
> 每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？
>
> 注意：给定 n 是一个正整数。

example

```
输入: 2
输出:2
解释:有两种方法可以爬到楼顶。

1.  1 阶 + 1 阶
2.  2 阶
```

```
输入:3
输出:3
解释:有三种方法可以爬到楼顶。

1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

code

```c++
int climbStairs(int n) {
    if (n <= 2) {
        return n;

    }
    int i1 = 1;
    int i2 = 2;
    for (int i = 2; i < n; i++) {
        int temp = i1 + i2;
        i1 = i2;
        i2 = temp;
    }

    return i2;
}
```

