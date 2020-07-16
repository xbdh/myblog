---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dynamic Programming-6 Longest Common Substring"
subtitle: ""
summary: "最长公共子序列"
authors: []
tags: ["Longest Common Substring"]
categories: ["Dynamic Programming"]
date: 2020-07-01T16:36:22+08:00
lastmod: 2020-07-01T16:36:22+08:00
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

## 1. longest common substring

> 求最长公共子串的长度

```c++
input:	s1="abdca", s2="cbda"
    
ouput:	2
   
explanation: "bd"
```

```c++
input:	s1="passport", s2="ppsspt"
    
ouput:	3
   
explanation: "ssp"
```

### brute-force

```c++
int LCSRecursive(string &s1, string &s2, int s1CurrentIndex, int s2CurrentIndex, int count) {
    if (s1CurrentIndex >= s1.length() || s2CurrentIndex >= s2.length()) {
        return count;
    }

    if (s1[s1CurrentIndex] == s2[s2CurrentIndex]) {
        count =  LCSRecursive(s1, s2, s1CurrentIndex + 1, s2CurrentIndex + 1, count+1);
    }
    //count清零
    int c1 = LCSRecursive(s1, s2, s1CurrentIndex + 1, s2CurrentIndex, 0);
    int c2 = LCSRecursive(s1, s2, s1CurrentIndex, s2CurrentIndex + 1, 0);

    return max(count, max(c1, c2));
}


int LCS(string s1, string s2) {

    return LCSRecursive(s1, s2, 0, 0, 0);
}
```

Time Complexity : *O*(*3^(M+N)* )

Space Complexity : *O*(*N+M*)

```c++
int LCS2(string s1, string s2) {

    int len1 = s1.length();
    int len2 = s2.length();
    int maxlen = 0;
    for (int i = 0; i < len1; i++) {
        for (int j = 0; j < len2; j++) {
            int len = 0;
            while (i + len < len1 && j + len < len2 && s1[i + len] == s2[j + len]) {
                len++;//暴力匹配
            }
            if (len > maxlen) {
                maxlen = len;
            }
        }
    }
    return maxlen;
}
```

### bottom-up

```c++
int LCSLength(string s1, string s2) {

    int len1 = s1.length();
    int len2 = s2.length();
    int maxLength = 0;

    //使用 d[i][j] 表示以 X[0,i-1]长度为i 与Y[0,j-1] 长度为j 最长公共子串的长度
    vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1));
    for (int i = 0; i <= len1; i++) {
       //可不要
        dp[0][i] = 0;
        dp[i][0] = 0;
    }
    for (int i = 1; i <= len1; i++) {
        for (int j = 1; j <= len2; j++) {
            if (s1[i - 1] == s2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
                maxLength = max(maxLength, dp[i][j]);
            }
        }
    }
    return maxLength;
}
```

Time Complexity : *O*(*M \* N*)

Space Complexity : *O*(*N \* M*)

![](./1-1.png)

![](./1-2.png)

![](./1-3.png)

![](./1-4.png)

![](./1-5.png)

![](./1-6.png)

![](./1-7.png)

![](./1-8.png)

## 2. longest common subsequence

> 求最长公共子序列

```c++
input:	s1="abdca", s2="cbda"
    
ouput:	3
   
explanation: "bda"
```

```c++
input:	s1="passport", s2="ppsspt"
    
ouput:	3
   
explanation: "ssp"
```

### brute-force

```c++
int LCSRecursive(string &s1, string &s2, int s1CurrentIndex, int s2CurrentIndex) {
    if (s1CurrentIndex >= s1.length() || s2CurrentIndex >= s2.length()) {
        return 0;
    }

    if (s1[s1CurrentIndex] == s2[s2CurrentIndex]) {
        return 1 + LCSRecursive(s1, s2, s1CurrentIndex + 1, s2CurrentIndex + 1);
    }
    //count清零
    int c1 = LCSRecursive(s1, s2, s1CurrentIndex + 1, s2CurrentIndex);
    int c2 = LCSRecursive(s1, s2, s1CurrentIndex, s2CurrentIndex + 1);

    return max(c1, c2);
}

```

Time Complexity : *O*(*2^(M+N)* )

Space Complexity : *O*(*N+M*)

### top-down

```c++
int LCS(string s1, string s2) {

    return LCSRecursive(s1, s2, 0, 0);
}

int LCSRecursive2(string &s1, string &s2, int s1CurrentIndex, int s2CurrentIndex, vector<vector<int>> dp) {
    if (s1CurrentIndex >= s1.length() || s2CurrentIndex >= s2.length()) {
        return 0;
    }

    if (dp[s1CurrentIndex][s2CurrentIndex] == -1) {
        if (s1[s1CurrentIndex] == s2[s2CurrentIndex]) {
            dp[s1CurrentIndex][s2CurrentIndex] = 1 + LCSRecursive2(s1, s2, s1CurrentIndex + 1, s2CurrentIndex + 1, dp);
        } else {
            int c1 = LCSRecursive2(s1, s2, s1CurrentIndex + 1, s2CurrentIndex, dp);
            int c2 = LCSRecursive2(s1, s2, s1CurrentIndex, s2CurrentIndex + 1, dp);

            dp[s1CurrentIndex][s2CurrentIndex] = max(c1, c2);
        }
    }
    return dp[s1CurrentIndex][s2CurrentIndex];
}

int LCS2(string s1, string s2) {
    vector<vector<int>> dp(s1.length(), vector<int>(s2.length(), -1));
    return LCSRecursive2(s1, s2, 0, 0, dp);
}

```

Time Complexity : *O*(*M \* N*)

Space Complexity : *O*(*N \* M*)

### bottom-up

```c++
int LCS3(string s1, string s2) {
    int len1 = s1.length();
    int len2 = s2.length();
    int maxLength = 0;

    //使用 d[i][j] 表示以 X[0,i-1]长度为i 与Y[0,j-1] 长度为j 最长公共子序列的长度
    vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1));
    for (int i = 0; i <= len1; i++) {
        //可不要
        dp[0][i] = 0;
        dp[i][0] = 0;
    }
    for (int i = 1; i <= len1; i++) {
        for (int j = 1; j <= len2; j++) {
            if (s1[i - 1] == s2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
            maxLength = max(maxLength, dp[i][j]);
        }
    }
    return maxLength;
}
```

Time Complexity : *O*(*M \* N*)

Space Complexity : *O*(*N \* M*)

![](./2-1.png)

![](./2-2.png)

![](./2-3.png)

![](./2-4.png)

![](./2-5.png)

![](./2-6.png)

![](./2-7.png)

![](./2-8.png)

##  3. minimum deletions or insertions to transform a string into another

> 给定字符串S1,S2,通过删除插入字符使得S1变成S2，求最小的删除插入数

```c++
input:	s1="abc",s2="fbc"
    
output:	1 deletions ,1 insertions
   
explanation: s1:delete(a),insert(f) ->s2
```

```c++
input:	s1="abdca",s2="cbda"
    
output:	2 deletions ,1 insertions
   
explanation: s1:delete(a,c),insert(c) ->s2
```

### bottom-up

```c++
int LCS3(string s1, string s2) {
    int len1 = s1.length();
    int len2 = s2.length();
    int maxLength = 0;

    //使用 d[i][j] 表示以 X[0,i-1]长度为i 与Y[0,j-1] 长度为j 最长公共子序列的长度
    vector<vector<int>> dp(len1 + 1, vector<int>(len2 + 1));
    for (int i = 0; i <= len1; i++) {
        //可不要
        dp[0][i] = 0;
        dp[i][0] = 0;
    }
    for (int i = 1; i <= len1; i++) {
        for (int j = 1; j <= len2; j++) {
            if (s1[i - 1] == s2[j - 1]) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
            maxLength = max(maxLength, dp[i][j]);
        }
    }
    return maxLength;
}

pair<int, int> minDeleteInsert(string s1, string s2) {
    int len1 = s1.length();
    int len2 = s2.length();
    int c = LCS3(s1, s2);
    return make_pair(len1 - c, len2 - c);
}
```

Time Complexity : *O*(*M \* N*)

Space Complexity : *O*(*N \* M*)

## 4. longest increasing subsequence

> 求最长递增子序列的长度

```c++
input:	[4, 2, 3, 6, 10, 1, 12]

output:	5
    
explanation:[2, 3, 6, 10, 12]
```

```c++
input:	[-4, 10, 3, 7, 15]

output:	4
    
explanation:[-4, 3, 7, 15]
```

### brute-force

```c++
int LISLengthRecursive(vector<int> &nums, int currentIndex, int previousIndex) {
    if (currentIndex >= nums.size()) {
        return 0;
    }
    int c1 = 0;

    //大于先前元素，包含在内
    if (previousIndex == -1 || nums[currentIndex] > nums[previousIndex]) {
        c1 = 1 + LISLengthRecursive(nums, currentIndex + 1, currentIndex);
    }

    //不包含
    int c2 = LISLengthRecursive(nums, currentIndex + 1, previousIndex);

    return max(c1, c2);
}

int LIS(vector<int> nums) {
    return LISLengthRecursive(nums, 0, -1);
}
```

Time Complexity : *O*(*2^N* )

Space Complexity : *O*(*N*)

### top-down

```c++
int LISLengthRecursive2(vector<int> &nums, int currentIndex, int previousIndex, vector<vector<int>> dp) {
    if (currentIndex >= nums.size()) {
        return 0;
    }
    //previousIndex的取值范围为[-1,n-1]转变为[0,n]
    if (dp[currentIndex][previousIndex + 1] == -1) {
        int c1 = 0;

        //大于先前元素，包含在内
        if (previousIndex == -1 || nums[currentIndex] > nums[previousIndex]) {
            c1 = 1 + LISLengthRecursive2(nums, currentIndex + 1, currentIndex, dp);
        }
        //不包含
        int c2 = LISLengthRecursive2(nums, currentIndex + 1, previousIndex, dp);

        dp[currentIndex][previousIndex + 1] = max(c1, c2);
    }
    return dp[currentIndex][previousIndex + 1];
}

int LIS2(vector<int> nums) {
    vector<vector<int>> dp(nums.size(), vector<int>(nums.size() + 1, -1));
    return LISLengthRecursive2(nums, 0, -1, dp);
}
```

Time Complexity : *O*(*N^2* )

Space Complexity : *O*(*N^2*)

### bottom-up

![](./4-1.png)

```c++
int LIS3(vector<int> nums) {
    vector<int> dp(nums.size());
    dp[0] = 1;
    int maxLength = 1;
    for (int i = 0; i < nums.size(); i++) {
        dp[i] = 1;
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j] && dp[i] <= dp[j]) {
                dp[i] = dp[j] + 1;
                maxLength = max(maxLength, dp[i]);
            }
        }
    }
    return maxLength;
}
```

Time Complexity : *O*(*N^2* )

Space Complexity : *O*(*N*)

![](./4-2.png)

![](./4-3.png)

![](./4-4.png)

![](./4-5.png)

![](./4-6.png)

![](./4-7.png)

## 5. max sum increasing subsequence

> 求 和最大的递增子序列
>
> 不同于题4

```c++
input:	[4, 1, 2, 6, 10, 1, 12]

output:	32
    
explanation:sum[4, 6, 10, 12]=32
    LIS：sum[1,2,6,10,12]=31,两者和不同
```

```c++
input:	[-4, 10, 3, 7, 15]

output:	25
    
explanation:sum[10, 15]=sum[3,7,5]=25
```

### brute-force

```c++
int maxSumISRecursive(const vector<int> &nums, int currentIndex, int previousIndex, int sum) {
    if (currentIndex >= nums.size()) {
        return sum;
    }
    int s1 = sum;

    if (previousIndex == -1 || nums[currentIndex] > nums[previousIndex]) {
        s1 = maxSumISRecursive(nums, currentIndex + 1, currentIndex, sum + nums[currentIndex]);
    }

    int s2 = maxSumISRecursive(nums, currentIndex + 1, previousIndex, sum);

    return max(s1, s2);
}

int maxSumIS(const vector<int> &nums) {
    return maxSumISRecursive(nums, 0, -1, 0);
}

```

Time Complexity : *O*(*2^N* )

Space Complexity : *O*(*N*)

### top-down

> 三维表或哈希表

### bottom-up

![](./5-1.png)

```c++

int maxSumIS2(const vector<int> &nums) {
    vector<int> dp(nums.size());
    dp[0] = nums[0];

    int maxSum = nums[0];
    for (int i = 0; i < nums.size(); i++) {
        dp[i] = nums[i];
        for (int j = 0; j < i; j++) {
            if (nums[i] > nums[j] && dp[i] < nums[j] + dp[j]) {
                dp[i] = dp[j] + nums[i];
                maxSum = max(maxSum, dp[i]);
            }
        }
    }
    return maxSum;
}
```

Time Complexity : *O*(*N^2* )

Space Complexity : *O*(*N*)

![](./5-2.png)

![](./5-3.png)

![](./5-4.png)

![](./5-5.png)

