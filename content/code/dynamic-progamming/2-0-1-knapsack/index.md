---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dynamic Programming-2 0-1 Knapsack"
subtitle: ""
summary: "0-1背包"
authors: []
tags: ["Dynamic Programming"]
categories: []
date: 2020-07-01T16:33:38+08:00
lastmod: 2020-07-01T16:33:38+08:00
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

## 1、0-1 Knapsack

> 给定N个物品的价值和重量，一个容量为C的背包。每个物品只能挑选一次且总容量不能超过C，求最大价值

```c++
input:	weights=[2, 3, 1, 4]
    	profits=[4, 5, 3, 7]
    	C=5
    
output:	10  
  
explanations:	1+4=5,3+7=10
```

### 暴力法

```c++
int knapsackRecursive(const vector<int> &weight, const vector<int> &profits, int capacity, int currentIndex) {
    if (capacity == 0 || currentIndex >= weight.size() || weight.size() != profits.size()) {
        return 0;
    }

    int profit1 = 0;

    if (weight[currentIndex] <= capacity) {
        profit1 = profits[currentIndex] +
                  knapsackRecursive(weight, profits, capacity - weight[currentIndex], currentIndex + 1);
    }

    int profit2 = knapsackRecursive(weight, profits, capacity, currentIndex + 1);

    return max(profit1, profit2);
}

int knapsack(const vector<int> &weight, const vector<int> &profits, int capacity) {
    return knapsackRecursive(weight, profits, capacity, 0);
}
```

Time Complexity : *O*(*2^N* )

Space Complexity : *O*(*N*)

### 自顶向下

```c++
int knapsackRecursive(const vector<int> &weight, const vector<int> &profits, int capacity, vector<vector<int>> &dp,
                       int currentIndex) {
    if (capacity == 0 || currentIndex >= weight.size()) {
        return 0;
    }

    if (dp[currentIndex][capacity] != -1) {
        return dp[currentIndex][capacity];
    }

    int profit1 = 0;

    if (weight[currentIndex] <= capacity) {
        profit1 = profits[currentIndex] +
                  knapsackRecursive(weight, profits, capacity - weight[currentIndex], dp, currentIndex + 1);
    }

    int profit2 = knapsackRecursive(weight, profits, capacity, dp, currentIndex + 1);
    dp[currentIndex][capacity] = max(profit1, profit2);

    return dp[currentIndex][capacity];
}

int knapsack(const vector<int> &weight, const vector<int> &profits, int capacity) {
    vector<vector<int>> dp(weight.size(), vector<int>(capacity + 1, -1));
    return knapsackRecursive(weight, profits, capacity, dp, 0);
}
```

Time Complexity : *O*(*N \* C*)

Space Complexity : *O*(*N \* C*)