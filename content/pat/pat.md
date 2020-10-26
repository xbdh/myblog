---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "PAT"
subtitle: ""
summary: "题解"
authors: []
tags: ["PAT"]
categories: ["PAT","interview"]
date: 2020-10-23T14:35:16+08:00
lastmod: 2020-10-23T14:35:16+08:00
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

### 1100. Mars Number(20)

### 1151. LCA in a Binary Tree (30分)

The lowest common ancestor (LCA) of two nodes U and V in a tree is the deepest node that has both U and V as descendants.

Given any two nodes in a binary tree, you are supposed to find their LCA.

**Input Specification:**

Each input file contains one test case. For each case, the first line gives two positive integers: M (≤ 1,000), the number of pairs of nodes to be tested; and N (≤ 10,000), the number of keys in the binary tree, respectively. In each of the following two lines, N distinct integers are given as the inorder and preorder traversal sequences of the binary tree, respectively. It is guaranteed that the binary tree can be uniquely determined by the input sequences. Then M lines follow, each contains a pair of integer keys U and V. All the keys are in the range of **int**.

**Output Specification:**

For each given pair of U and V, print in a line `LCA of U and V is A.` if the LCA is found and `A` is the key. But if `A` is one of U and V, print `X is an ancestor of Y.` where `X` is `A` and `Y` is the other node. If U or V is not found in the binary tree, print in a line `ERROR: U is not found.` or `ERROR: V is not found.` or `ERROR: U and V are not found.`.

**Sample Input:**

```c++
6 8
7 2 3 4 6 5 1 8
5 3 7 2 6 4 8 1
2 6
8 1
7 9
12 -3
0 8
99 99
```

**Sample Output:**

```cpp
LCA of 2 and 6 is 3.
8 is an ancestor of 1.
ERROR: 9 is not found.
ERROR: 12 and -3 are not found.
ERROR: 0 is not found.
ERROR: 99 and 99 are not found.
```

**code**

```c++
#include <iostream>
#include <vector>
#include <map>
using namespace std;
map<int, int> pos;
vector<int> in, pre;
void lca(int inl, int inr, int preRoot, int a, int b) {
    if (inl > inr) return;
    int inRoot = pos[pre[preRoot]], aIn = pos[a], bIn = pos[b];
    if (aIn < inRoot && bIn < inRoot)
        lca(inl, inRoot-1, preRoot+1, a, b);
    else if ((aIn < inRoot && bIn > inRoot) || (aIn > inRoot && bIn < inRoot))
        printf("LCA of %d and %d is %d.\n", a, b, in[inRoot]);
    else if (aIn > inRoot && bIn > inRoot)
        lca(inRoot+1, inr, preRoot+1+(inRoot-inl), a, b);
    else if (aIn == inRoot)
            printf("%d is an ancestor of %d.\n", a, b);
    else if (bIn == inRoot)
            printf("%d is an ancestor of %d.\n", b, a);
}
int main() {
    int m, n, a, b;
    scanf("%d %d", &m, &n);
    in.resize(n + 1), pre.resize(n + 1);
    for (int i = 1; i <= n; i++) {
        scanf("%d", &in[i]);
        pos[in[i]] = i;
    }
    for (int i = 1; i <= n; i++) scanf("%d", &pre[i]);
    for (int i = 0; i < m; i++) {
        scanf("%d %d", &a, &b);
        if (pos[a] == 0 && pos[b] == 0)
            printf("ERROR: %d and %d are not found.\n", a, b);
        else if (pos[a] == 0 || pos[b] == 0)
            printf("ERROR: %d is not found.\n", pos[a] == 0 ? a : b);
        else
            lca(1, n, 1, a, b);
    }
    return 0;
}
```

