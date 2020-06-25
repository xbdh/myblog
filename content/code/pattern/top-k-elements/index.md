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
struct Point {
    int x, y;

    Point(int _x, int _y) : x(_x), y(_y) {};
};

int distance(Point p) {
    return p.x * p.x + p.y + p.y;
}

bool operator<(Point a, Point b) {
    //return a.x*a.x +a.y*a.y > b.x*b.x+b.y+b.y;
    return distance(a) < distance(b);
}


vector<Point> findKthClosestPoints(const vector<Point> &Points, int k) {
    priority_queue<Point> maxHeap;

    vector<Point> result;
    for (int i = 0; i < k; i++) {
        maxHeap.push(Points[i]);
    }

    for (int i = k; i < Points.size(); i++) {
        if (distance(Points[i]) < distance(maxHeap.top())) {
            maxHeap.pop();
            maxHeap.push(Points[i]);
        }
    }
    for (int i = 0; i < k; i++) {
        result.push_back(maxHeap.top());
        maxHeap.pop();
    }
    return result;
}

```

Time Complexity : *O*(*N*  * log *K*)

Space Complexity : *O*(*K*)

## 5、connect ropes

> 把N段不同绳子连接成一段长绳子，使得cost最小

> 连接两段绳子的cost= 两段绳子的长度和

```c++
input: [1, 3, 11, 5]

output: 33
    
explanations: cost1: 1 + 3 = 4; cost2: 4 + 5 = 9 ; cost3 :9 + 11 =20 ;totalcost: 4 + 9 + 20 = 33
```

```c++
input: [3, 4, 5, 6]

output: 36
    
explanations: cost1: 3 + 4 = 7; cost2: 5 + 6 = 11 ; cost3 :7 + 11 =18 ;totalcost: 7 + 11 + 18 = 36
```

```c++
input: [1, 3, 11, 5, 2]

output: 42
    
explanations: cost1: 1 + 2 = 3; cost2: 3 + 3 = 6 ; cost3 :6 + 5 =11 ;cost4 :11 + 11 =22 ; totalcost: 3 +6  + 11 + 12 = 42
```

code:

```c++
int minimumCostConnectRopes(const vector<int> &nums) {

    int result = 0;
    int temp = 0;

    priority_queue<int, vector<int>, greater<int>> minHeap;

    for (int i = 0; i < nums.size(); i++) {
        minHeap.push(nums[i]);
    }

    while (minHeap.size() > 1) {
        temp = minHeap.top();
        minHeap.pop();
        temp += minHeap.top();
        minHeap.pop();
        result += temp;
        minHeap.push(temp);
    }
    return result;
}
```

Time Complexity : *O*(*N*  * log *N*)

Space Complexity : *O*(*N*)

## 6、top k frequent numbers

> 给定未排序数组和K值，求出现次数前K的数

```c++
input: [1, 3, 5, 12, 11, 12, 11], k=2
    
output:	[12, 11]

explanations: 12(2), 11(2) , 其他1次
```

```c++
input: [5, 12, 11, 3, 11], k=2
    
output:	[11, 5] 或[11, 12] 或[11, 3]

explanations: 11(2), 其他1次
```

```c++
struct cmp_greater {
    bool operator()(const pair<int, int> &x, const pair<int, int> &y) {
        return x.second > y.second;
    }
};

vector<int> findTopKFrequencyNumbers(const vector<int> &nums, int k) {
    unordered_map<int, int> numFrequencyMap;
    for (auto n:nums) {
        numFrequencyMap[n]++;
    }

    priority_queue<pair<int, int>, vector<pair<int, int>>, cmp_greater> minHeap;

    for (auto entry: numFrequencyMap) {
        minHeap.push(entry);
        if (minHeap.size() > k) {
            minHeap.pop();
        }
    }

    vector<int> topNumbers;
    while (!minHeap.empty()) {
        topNumbers.push_back(minHeap.top().first);
        minHeap.pop();
    }

    return topNumbers;
}
```

Time Complexity : *O*(*N* + *N*  * log *K*)

Space Complexity : *O*(*N*)

## 7、frequency sort

> 给定字符串，按照字符出现的次数降序排列

```c++
input:	"Programming"
    
output:	"rrggmmPiano"
```

```c++
input:	"abcbab"
    
output:	"bbbaac"
```

code:

```c++
struct cmp_smaller {
    bool operator()(const pair<char, int> &x, const pair<char, int> &y) {
        return x.second < y.second;
    }
};

string sortCharacterByFrequency(const string &str) {
    unordered_map<char, int> characterFrequencyMap;
    for (char chr:str) {
        characterFrequencyMap[chr]++;
    }

    priority_queue<pair<int, int>, vector<pair<int, int>>, cmp_smaller> maxHeap;
    for (auto entry :characterFrequencyMap) {
        maxHeap.push(entry);
    }

    string sortedString = "";
    while (!maxHeap.empty()) {
        auto entry = maxHeap.top();
        maxHeap.pop();
        for (int i = 0; i < entry.second; i++) {
            sortedString += entry.first;
        }
    }

    return sortedString;
}

```

Time Complexity : *O*(*N*  * log *N*)

Space Complexity : *O*(*N*)

## 8、kth largest number in a stream

> 设计一个类，求数据流中的最大值

## 9、k closest numbers

> 给定排序的数组，及整数K和X。求数组中K个接近X的数，将返回的数排序，X不一定在原数组中。

```c++
input:	[5, 6, 7, 8, 9] , k=3 ,x=7
    
output:	[6, 7 , 8]
```

```c++
input:	[2 ,4 ,5 ,6 ,9] ,k=3 ,x=6
    
output:	[4, 5, 6]
```

```c++
input:	[2, 4, 5, 6, 9] ,k=3 ,x=10
    
output:	[5, 6, 9]
```

