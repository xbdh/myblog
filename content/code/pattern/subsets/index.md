---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "pattern-11 Subsets"
subtitle: ""
summary: "子集"
authors: []
tags: ["pattern"]
categories: []
date: 2020-05-23T17:53:35+08:00
lastmod: 2020-05-23T17:53:35+08:00
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

解决排列和组合问题，使用广度优先算法

## 2、subsets

>给定不含重复元素的集合，求其所有不同的子集

```c++
input:	[1, 3]

output:	[], [1], [3], [1, 3]
```

````c++
input:	[1, 5, 3]

output:	[], [1], [3], [5] ,[1, 3], [1, 5], [3 ,5],[1 ,3 ,5]
````

code:

```c++
vector<vector<int>> findSubsets(const vector<int>&nums){
    vector<vector<int>> subsets;
    subsets.push_back(vector<int>());
	//取已经有的子集，插入新的元素，生成新的子集
    for(auto currentNumber: nums){
        int n=subsets.size();
        for(int i=0;i<n;i++){
            vector<int> set(subsets[i]);
            set.push_back(currentNumber);
            subsets.push_back(set);
        }
    }
    return subsets;
}
```

Time Complexity : *O*(2^*N*)

Space Complexity : *O*(2^*N*)

## 3、subsets with Duplicates

>  给定含重复元素的集合，求其所有不同的子集

```c++
input:	[1, 3, 3]

output:	[], [1], [3], [1, 3], [3, 3], [1, 3, 3]
```

```c++
input:	[1, 5, 3, 3]

output:	[], [1], [3], [5], [1, 5] ,[1, 3], [1, 5, 3], [3, 3], [1, 3, 3],[5 ,3], [3,3,5],[1, 5, 3, 3]
```

code:

```c++
vector<vector<int>> findSubsets(vector<int> &nums) {
    sort(nums.begin(), nums.end());
    vector<vector<int>> subsets;
    subsets.push_back(vector<int>());
    int startIndex = 0;
    int endIndex = 0;
    
    //当遇到重复的元素时，取上一步生成的子集，插入新的元素，生成新的子集
    for (int i = 0; i < nums.size(); i++) {
        startIndex = 0;
        if (i > 0 && nums[i] == nums[i - 1]) {
            startIndex = endIndex + 1;
        }
        endIndex = subsets.size() - 1;
        for (int j = startIndex; j <= endIndex; j++) {
            vector<int> set(subsets[j]);
            set.push_back(nums[i]);
            subsets.push_back(set);
        }
    }
    return subsets;
}
```

![](./3-1.png)

Time Complexity : *O*(2^*N*)

Space Complexity : *O*(2^*N*)

## 4、permutations

> 给定不含重复元素的集合，求其所有排列

```c++
input:	[1, 3, 5]

output:	[1, 3, 5], [1 ,5, 3], [3, 5, 1], [3, 1, 5], [5, 1, 3], [5, 3, 1]
```

code:

```c++
vector<vector<int>> findPermutations(vector<int> &nums) {
    vector<vector<int>> result;
    queue<vector<int>> permutations;

    permutations.push(vector<int>());
    for (auto currentNumber:nums) {
        int n = permutations.size();
        for (int i = 0; i < n; i++) {
            vector<int> oldPermutation = permutations.front();
            permutations.pop();
            
            //添加currentNumber ，到所有的position
            for (int j = 0; j <= oldPermutation.size(); j++) {
                vector<int> newPermutations(oldPermutation);
                newPermutations.insert(newPermutations.begin() + j, currentNumber);

                if (newPermutations.size() == nums.size()) {
                    result.push_back(newPermutations);
                } else {
                    permutations.push(newPermutations);
                }
            }
        }
    }
    return result;
}
```

![](./4-1.png)

递归的方法：

```c++
void generatePermutationsRecursive(vector<int> &nums, int numsIndex,
                                   vector<int> &currentPermutation, vector<vector<int>> &result) {
    if (numsIndex == nums.size()) {
        result.push_back(currentPermutation);
    } else {
        for (int i = 0; i <= currentPermutation.size(); i++) {
            vector<int> newPermutation(currentPermutation);
            newPermutation.insert(newPermutation.begin() + i, nums[numsIndex]);
            generatePermutationsRecursive(nums, numsIndex + 1, newPermutation, result);

        }
    }
}

vector<vector<int>> generatePermutation(vector<int> &nums) {
    vector<vector<int>> result;
    vector<int> currentPermutation;
    
    generatePermutationsRecursive(nums, 0, currentPermutation, result);
    return result;
}
```



Time Complexity : *O*(*N* * *N !*)

Space Complexity : *O*(*N* * *N !*)

## 5、string permutation by changing case

> 给定字符串，保留原序列，只改变字母的大小写，求所有排列

```c++
input:	"ad52"
  
ouput:	"ad52" ,"Ad52", "aD52", "AD52"
```

```c++
input:	"ab7c"
  
ouput:	"ab7c", "Ab7c","aB7c","ab7C","AB7c","Ab7C","aB7C","AB7C"
```

code:

```c++
vector<string> findLetterCaseStringPermutation(const string &str) {
    vector<string> permutations;
    if (str == "") {
        return permutations;
    }
    permutations.push_back(str);

    for (int i = 0; i < str.length(); i++) {
        if (isalpha(str[i])) {
            int n = permutations.size();
            for (int j = 0; j < n; j++) {
                vector<char> chs(permutations[j].begin(), permutations[j].end());
                if (isupper(chs[i])) {
                    chs[i] = tolower(chs[i]);
                } else {
                    chs[i] = toupper(chs[i]);
                }

                permutations.push_back(string(chs.begin(), chs.end()));
            }
        }
    }
    return permutations;
}
```

![](./5-1.png)

Time Complexity : *O*(*N* * 2^ *N* )

Space Complexity : *O*(*N* * 2^ *N* )

## 6、balanced parentheses

> 给定N，求n对（）的合理的组合

```c++
input: N=2
    
output:	(()) ,()()
```

```c++
input: N=3
    
output:	((())) ,()()(), (())(), ()(()), ((),())
```

code:

```c++
struct Parenthesese {
    string str;
    int openCount;
    int closeCount;

    Parenthesese(const string &str, int openCount, int closeCount) : str(str), openCount(openCount),
                                                                     closeCount(closeCount) {};
};

vector<string> generateValidParentheses(int num) {
    vector<string> result;
    queue<Parenthesese> queue;
    queue.push({"", 0, 0});

    while (!queue.empty()) {
        Parenthesese ps = queue.front();
        queue.pop();

        if (ps.openCount == num && ps.closeCount == num) {
            result.push_back(ps.str);
        } else {
            if (ps.openCount < num) {
                queue.push({ps.str + "(", ps.openCount + 1, ps.closeCount});
            }
            if (ps.openCount > ps.closeCount) {
                queue.push({ps.str + ")", ps.openCount, ps.closeCount + 1});
            }
        }
    }
    return result;
}
```

Time Complexity : ![](./6-1.png)

Space Complexity : *O*(*N* * 2^ *N* )

递归方法

```c++
void generateValidParenthesesRecursive(int num, int openCount, int closeCount, int stringIndex,
                                       vector<char> &parenthesesString, vector<string> &result) {
    if (openCount == num && closeCount == num) {
        result.push_back(string(parenthesesString.begin(), parenthesesString.end()));

    } else {
        if (openCount < num) {
            parenthesesString[stringIndex] = '(';
            generateValidParenthesesRecursive(num, openCount+1, closeCount , stringIndex + 1, parenthesesString,
                                              result);
        }
        if (openCount > closeCount) {
            parenthesesString[stringIndex] = ')';
            generateValidParenthesesRecursive(num, openCount, closeCount + 1, stringIndex + 1, parenthesesString,
                                              result);
        }
    }
}

vector<string> generateValidParentheses2(int num) {
    vector<string> result;
    vector<char> parenthesesString(2 * num);
    generateValidParenthesesRecursive(num, 0, 0, 0, parenthesesString, result);
    return result;

}
```

## 7、unique generalized abbreviations

> 没看懂题目

