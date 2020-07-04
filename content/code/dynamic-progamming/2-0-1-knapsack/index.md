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

![](./1-1.png)

```c++
int knapsackRecursive(const vector<int> &weight, const vector<int> &profits, int capacity, int currentIndex) {
    if (capacity <= 0 || currentIndex >= weight.size() || weight.size() != profits.size()) {
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

有重复

![](./1-2.png)

```c++
int knapsackRecursive(const vector<int> &weight, const vector<int> &profits, int capacity, vector<vector<int>> &dp,
                       int currentIndex) {
    if (capacity <= 0 || currentIndex >= weight.size()) {
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

### 自底向上

![](./1-3.png)

![](./1-4.png)

![](./1-5.png)

![](./1-6.png)

![](./1-7.png)

![](./1-8.png)

![](./1-9.png)

![](./1-10.png)

![](./1-11.png)

![](./1-12.png)

![](./1-13.png)

![](./1-14.png)

![](./1-15.png)



```c++
int knapsack3(const vector<int> &weight, const vector<int> &profits, int capacity) {
    if (capacity <= 0 || profits.empty() || profits.size() != weight.size()) {
        return 0;
    }

    int n = profits.size();
    vector<vector<int>> dp(n, vector<int>(capacity + 1));

    for (int i = 0; i < n; i++) {
        dp[i][0] = 0;
    }

    //如果第一个物品小于物品重量，放置
    for (int j = 0; j <= capacity; j++) {
        if (weight[0] <= j) {
            dp[0][j] = profits[0];
        }
    }

    //之后的物品
    for (int i = 1; i < n; i++) {
        for (int j = 1; j <= capacity; j++) {
            int profit1 = 0, profit2 = 0;

            if (weight[i] <= j) {
                profit1 = profits[i] + dp[i - 1][j - weight[i]];
            }
            profit2 = dp[i - 1][j];
            dp[i][j] = max(profit1, profit2);
        }
    }
	
    return dp[n - 1][capacity];
}
```

Time Complexity : *O*(*N \* C*)

Space Complexity : *O*(*N \* C*)

输出选择的物品：

> 当某个物品不选择时，dp的值来源于正上方，且相等。若不相等，则此物品被选中。

![](./1-16.png)



```c++
void printSelectElements(const vector<int> &weight, const vector<int> &profits, int capacity, vector<vector<int>> &dp) {
    int n = weight.size();
    int totalProfit = dp[n - 1][capacity];
    cout << "--------";
    //不能等于0，会越界
    for (int i = n - 1; i > 0; i--) {
        //最值与正上方比较
        if (totalProfit != dp[i - 1][capacity]) {

            cout << weight[i] << " ";
            cout << "index=" << i << " ";
            capacity -= weight[i];
            totalProfit -= profits[i];
        }
    }
    //判断第一个元素是否被选中
    if (totalProfit != 0) {
        cout << weight[0];
        cout << "index=" << 0 << " ";
    }
    cout << "--------";
}
```

优化一：

```c++
int knapsack4(const vector<int> &weight, const vector<int> &profits, int capacity) {
    if (capacity <= 0 || profits.empty() || profits.size() != weight.size()) {
        return 0;
    }

    int n = profits.size();
    //当前dp的值，只与上一行有关，两行就行，覆盖即可，
    vector<vector<int>> dp(2, vector<int>(capacity + 1));


    //如果第一个物品小于物品重量，放置
    for (int j = 0; j <= capacity; j++) {
        if (weight[0] <= j) {
            dp[0][j] = dp[1][j] = profits[0];
        }
    }

    //之后的物品
    for (int i = 1; i < n; i++) {
        for (int j = 1; j <= capacity; j++) {
            int profit1 = 0, profit2 = 0;

            if (weight[i] <= j) {
                profit1 = profits[i] + dp[(i - 1) % 2][j - weight[i]];
            }
            profit2 = dp[(i - 1) % 2][j];
            dp[i % 2][j] = max(profit1, profit2);
        }
    }
    return dp[(n - 1) % 2][capacity];
}
```

Time Complexity : *O*(*N \* C*)

Space Complexity : *O*(*C*)

优化二：

```c++
int knapsack5(const vector<int> &weight, const vector<int> &profits, int capacity) {
    if (capacity <= 0 || profits.empty() || profits.size() != weight.size()) {
        return 0;
    }

    int n = profits.size();
    vector<int> dp(capacity + 1);


    //如果第一个物品小于物品重量，放置
    for (int j = 0; j <= capacity; j++) {
        if (weight[0] <= j) {
            dp[j] = profits[0];
        }
    }

    //之后的物品
    for (int i = 1; i < n; i++) {
        //顺序改变防止覆盖
        for (int j = capacity; j >= 0; j--) {
            int profit1 = 0, profit2 = 0;

            if (weight[i] <= j) {
                profit1 = profits[i] + dp[j - weight[i]];
            }
            profit2 = dp[j];
            dp[j] = max(profit1, profit2);
        }
    }
    return dp[capacity];
}
```

Time Complexity : *O*(*N \* C*)

Space Complexity : *O*(*C*)

## 