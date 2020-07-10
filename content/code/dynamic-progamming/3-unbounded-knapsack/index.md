---


# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dynamic Programming-3 Unbounded Knapsack"
subtitle: ""
summary: "无界限背包"
authors: []
tags: ["Dynamic Programming"]
categories: []
date: 2020-07-01T16:34:12+08:00
lastmod: 2020-07-01T16:34:12+08:00
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

## 1.unbounded knapsack

> 给定N个物品的价值和重量，一个容量为C的背包。每个物品不限次数且总容量不能超过C，求最大价值

```c++
input:	weights=[1, 2, 3]
    	profits=[10, 20, 50]
    	C=5
    
output:	80
  
explanations:	2*15+30=80
```

### Brute-force

```c++
int knapsackRecursive(const vector<int> &weight, const vector<int> &profits, int capacity, int currentIndex) {
    if (capacity <= 0 || currentIndex >= weight.size() || weight.size() != profits.size()) {
        return 0;
    }

    int profit1 = 0;
    //选中之后，currentIndex不增加
    if (weight[currentIndex] <= capacity) {
        profit1 = profits[currentIndex] +
                  knapsackRecursive(weight, profits, capacity - weight[currentIndex], currentIndex );
    }

    int profit2 = knapsackRecursive(weight, profits, capacity, currentIndex + 1);

    return max(profit1, profit2);
}

int knapsack(const vector<int> &weight, const vector<int> &profits, int capacity) {
    return knapsackRecursive(weight, profits, capacity, 0);
}
```

#### Complexity

Time Complexity : *O*(*2^(N+C)* )

Space Complexity : *O*(*N + C*)

### Top-down

