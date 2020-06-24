---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Pattern-14 Top K Elements"
subtitle: ""
summary: "前K个元素"
authors: []
tags: ["pattern"]
categories: []
date: 2020-05-23T17:56:30+08:00
lastmod: 2020-05-23T17:56:30+08:00
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

最大/最小/出现次数 的 第/前 K个元素，常用heap。

## 2、top k numbers

> 给定未排序数组和K值，求前K个大的元素

```c++
input:	[3, 1, 5, 12, 2, 11] ,K=3
    
output: [5, 12, 11]
```

```c++
input:	[5, 12, 11, -1, 12] ,K=3
    
output: [5, 12, 12]
```

```c++
vector<int> findLargestNumbers(const vector<int> &nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    vector<int> result;
    for (int i = 0; i < k; i++) {
        minHeap.push(nums[i]);
    }

    for (int i = k; i < nums.size(); i++) {
        if (nums[i] > minHeap.top()) {
            minHeap.pop();
            minHeap.push(nums[i]);
        }
    }
    for (int i = 0; i < k; i++) {
        result.push_back(minHeap.top());
        minHeap.pop();
    }
    return result;
}

```

Time Complexity : *O*(*N*  * log *K*)

Space Complexity : *O*(*K*)

## 3、kth smallest number

> 给定未排序数组和K值，求第K个小的元素

```c++
input:	[1, 5, 12, 2, 11, 5] ,K=3
    
output: 5
    
explanations: 1 2 5 5 11 12
```

```c++
input:	[1, 5, 12, 2, 11, 5] ,K=4
    
output: 5
    
explanations: 1 2 5 5 11 12
```

```c++
input:	[5, 11, 12, -1, 12] ,K=3
    
output: 11
    
explanations: -1 5  11 12 12
```

code:

```c++
int kthSmallestNumber(const vector<int> &nums, int k) {
    priority_queue<int> maxHeap;
    for (int i = 0; i < k; i++) {
        maxHeap.push(nums[i]);
    }

    for (int i = k; i < nums.size(); i++) {
        if (nums[i] < maxHeap.top()) {
            maxHeap.pop();
            maxHeap.push(nums[i]);
        }
    }
    return maxHeap.top();
}
```

Time Complexity : *O*(*N*  * log *K*)

Space Complexity : *O*(*K*)

使用小顶堆：

code：

```c++
int kthSmallestNumberUseMinHeap(const vector<int> &nums, int k) {
    priority_queue<int, vector<int>, greater<int>> minHeap;
    
    for (int i = 0; i < nums.size(); i++) {
        minHeap.push(nums[i]);
    }

    for (int i = 0; i < k - 1; i++) {
        minHeap.pop();
    }
    
    return minHeap.top();
}
```

Time Complexity : *O*(*N*  + *K* log *N*)

Space Complexity : *O*(*N*)

## 4、k closest points to the origin

> 给定一组二位坐标点和K值，求前K个离原点最近的的点

```c++
input:	[[1, 2], [1, 3]], k=1
    
output:	[[1, 2]]
```

```c++
input:	[[1, 3], [3, 4], [2, -1]], k=2
    
output:	[[1, 3],[2, -1]]
```

code:

```c++

```

