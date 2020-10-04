---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Pattern-15 K Way Merge"
subtitle: ""
summary: "多路归并排序"
authors: []
tags: ["多路归并排序"]
categories: ["pattern"]
date: 2020-05-23T17:56:50+08:00
lastmod: 2020-05-23T17:56:50+08:00
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

## 1、introduction

解决k个有序数组的合并、最值问题。用heap。

## 2、merge k sorted lists

> 给定K组有序链表，合成一个有序链表

```c++
input:	l1=[2, 6, 8], l2=[3, 6, 7], l3=[1, 3, 4]
    
output:	[1, 2, 3, 3, 4, 6, 6, 7, 8]
```

```c++
input:	l1=[5, 8, 9] ,l2=[1, 7]
    
output:	[1, 5, 7, 8, 9]
```

code：

```c++
struct ListNode {
    int val;
    ListNode *next;

    ListNode(int data) : val(data), next(NULL) {};
};

struct cmp_greater {
    bool operator()(const ListNode *x, const ListNode *y) {
        return x->val > y->val;
    }
};

ListNode *merge(const vector<ListNode *> &lists) {

    priority_queue<ListNode *, vector<ListNode *>, cmp_greater> minHeap;

    for (auto root:lists) {
        if (root != NULL) {
            minHeap.push(root);
        }
    }

    ListNode *resultHead = NULL;
    ListNode *resultTail = NULL;

    while (!minHeap.empty()) {
        ListNode *node = minHeap.top();
        minHeap.pop();
        if (resultHead == NULL) {
            resultHead = node;
            resultTail = node;
        } else {
            resultTail->next = node;
            resultTail = resultTail->next;
            //resultTail=node;
        }

        if (node->next != NULL) {
            minHeap.push(node->next);
        }
    }

    return resultHead;
}
```

Time Complexity : *O*(*N \* log K*)

Space Complexity : *O*(*K*)

## 3、kth smallest number in m sorted lists

> 给定M个有序数组，求所有数组中第K小的元素

```c++
input:	l1=[2, 6, 8], l2=[3, 6, 7], l3=[1, 3, 4],K=5
    
output:	4

explanations:[1, 2, 3, 3, 4, 6, 6, 7, 8] 5th->4
```

```c++
input: l1=[5, 8, 9], l2=[1, 7],K=3
   
output:	7
    
explanations:[1, 5, 7, 8, 9] 3th->7
```

code:

```c++
struct cmp_greater {
    bool operator()(const pair<int, pair<int, int>> &x, const pair<int, pair<int, int>> &y) {
        return x.first > y.first;
    }
};

int kthSmallest(const vector<vector<int>> &lists, int k) {
    //pair<int,pair<int,int>>> 值，值所在第几个数组，值得索引
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, cmp_greater> minHeap;
    for (int i = 0; i < lists.size(); i++) {
        if (!lists[i].empty()) {
            minHeap.push({lists[i][0], {i, 0}});
        }
    }

    int numberCount = 0, result = 0;

    while (!minHeap.empty()) {
        auto node = minHeap.top();
        minHeap.pop();

        result = node.first;

        if (++numberCount == k) {
            break;
        }

        //下一个node
        node.second.second++;

        //node 所在链表还有元素
        if (lists[node.second.first].size() > node.second.second) {
            node.first = lists[node.second.first][node.second.second];
            minHeap.push(node);
        }
    }

    return result;
}
```

Time Complexity : *O*(*K \* log M*)

Space Complexity : *O*(*M*)

相似问题1：

> 求m个有序数组、链表的平均值

> 解：此时k=N/2

相似问题1：

> 合并m个有序数组

> 解：同此题，要记录元素所在数组及索引

## 4、kth smallest number in a sorted matrix

> 给定矩阵和K值，每行每列增序，求第K小的数

```c++
input:	maxtrix=[
    [2, 6, 8],
    [3, 7, 10],
    [5, 8, 11]
	], k=5

output:	7
```

code:

```c++
struct cmp_greater {
    bool operator()(const pair<int, pair<int, int>> &x, const pair<int, pair<int, int>> &y) {
        return x.first > y.first;
    }
};

int kthSmallest(const vector<vector<int>> &matrix, int k) {
    //pair<int,pair<int,int>>> 值，值所在第几个数组，值得索引
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, cmp_greater> minHeap;

    //每行第一个元素放入heap，且不需要超过k个元素
    for (int i = 0; i < matrix.size() && i < k; i++) {
        minHeap.push({matrix[i][0], {i, 0}});
    }

    int numberCount = 0, result = 0;

    while (!minHeap.empty()) {
        auto node = minHeap.top();
        minHeap.pop();

        result = node.first;

        if (++numberCount == k) {
            break;
        }

        //下一个node
        node.second.second++;

        //node 所在链表还有元素
        if (matrix.size() > node.second.second) {
            node.first = matrix[node.second.first][node.second.second];
            minHeap.push(node);
        }
    }

    return result;
}
```

Time Complexity : *O*(*min(K,N)* + *K \* log N*)

Space Complexity : *O*(*N*)

二分查找的方法：

code：

```c++
待看
```



## 5、smallest number range

> 给定m个有序数组，求长度最小的范围区间，使得区间包含每个数组至少一个元素

```c++
input:	l1=[1, 5, 8], l2=[4, 12], l3=[7, 8, 10]
    
output:	[4, 7]

explanation: l1:->5,l2:->4, l3:->7
```

```c++
input:	l1=[1, 9], l2=[4, 12], l3=[7, 10, 16]
    
output:	[9, 12]

explanation: l1:->9 ,l2:->12, l3:->10
```

code:

```c++
struct cmp_greater {
    bool operator()(const pair<int, pair<int, int>> &x, const pair<int, pair<int, int>> &y) {
        return x.first > y.first;
    }
};

pair<int, int> smallestRange(const vector<vector<int>> &lists) {
    //pair<int,pair<int,int>>> 值，值所在第几个数组，值得索引
    priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, cmp_greater> minHeap;

    int rangeStart = 0, rangeEnd = INT_MAX;
    int currentMaxNumber = INT_MIN;

    for (int i = 0; i < lists.size(); i++) {
        if (!lists[i].empty()) {
            minHeap.push({lists[i][0], {i, 0}});
            //minHeap.push(make_pair(lists[i][0], make_pair(i, 0)));
            currentMaxNumber = max(lists[i][0], currentMaxNumber);
        }
    }


    while (minHeap.size() == lists.size()) {
        auto node = minHeap.top();
        minHeap.pop();

        if (rangeEnd - rangeStart > currentMaxNumber - node.first) {
            rangeStart = node.first;
            rangeEnd = currentMaxNumber;
        }

        //下一个node
        node.second.second++;

        //node 所在链表还有元素
        if (lists[node.second.first].size() > node.second.second) {
            node.first = lists[node.second.first][node.second.second];
            minHeap.push(node);
            currentMaxNumber = max(currentMaxNumber, node.first);
        }
    }

    return make_pair(rangeStart, rangeEnd);
}
```

Time Complexity : *O*(*N \* log M*)

Space Complexity : *O*(*M*)

## 6、k pair with largest sums

> 给定两个降序数组和k值，求和最大的K对数(每个数组各一个值，值允许重复)

```c++
input:	l1=[9, 8, 2], l2=[6, 3, 1], k=3
    
output:	[9, 3],[9, 6],[8, 6]

explanation: 和最大的三组
```

```c++
input:	l1=[5, 2, 1], l2=[2, -1], k=3
    
output:	[5, 2],[5, -1], [2,2]
```

code:

```c++
struct cmp_greater_sum{
    bool operator()(const pair<int,int> &x,const pair<int,int> &y){
        return x.first+x.second>y.second+y.first;
    }
};

vector<vector<int>> klargestPairs(const vector<int> &num1,const vector<int> &num2,int k) {
    priority_queue<pair<int,int>,vector<pair<int,int>>,cmp_greater_sum> minHeap;

    for(int i=0;i<num1.size();i++){
        for(int j=0;i<num2.size();j++){
            if(minHeap.size()<k){
                minHeap.push(make_pair(num1[i],num2[j]));
            }else{
                if(num1[i]+num2[j]<minHeap.top().first+minHeap.top().second){
                    break;
                }else{
                    minHeap.pop();
                    minHeap.push(make_pair(num1[i],num2[j]));
                }
            }
        }
    }

    vector<vector<int>> result;
    while(!minHeap.empty()){
        result.push_back({minHeap.top().first,minHeap.top().second});
        minHeap.pop();
    }
    return result;
}
```

Time Complexity : *O*(*N \* M \* log K*)

Space Complexity : *O*(*K*)