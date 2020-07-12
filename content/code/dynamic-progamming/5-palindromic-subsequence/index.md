---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Dynamic Programming-5 Palindromic Subsequence"
subtitle: ""
summary: "回文子序列"
authors: []
tags: ["Palindromic Subsequence"]
categories: ["Dynamic Programming"]
date: 2020-07-01T16:35:47+08:00
lastmod: 2020-07-01T16:35:47+08:00
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

## 1. longest Palindromic Subsequence

> 求最长回文子序列(不连续)的长度

```c++
input:	"bbbab"
    
output:	4
    
explanation: "bbbb"
```

```c++
input:	"cbbd"
    
output:	2
    
explanation: "bb"
```

### Brute-force

```c++
int longestPalindromicSubsequenceRecursive(const string &s, int startIndex, int endIndex) {
    if (startIndex > endIndex) {
        return 0;
    }
    //只有一个字符
    if (startIndex == endIndex) {
        return 1;
    }

    if (s[startIndex] == s[endIndex]) {
        return 2 + longestPalindromicSubsequenceRecursive(s, startIndex + 1, endIndex - 1);

    }
    //从头或尾跳过一个字符

    int c1 = longestPalindromicSubsequenceRecursive(s, startIndex + 1, endIndex);
    int c2 = longestPalindromicSubsequenceRecursive(s, startIndex, endIndex - 1);
    return max(c1, c2);
}

int LPS(string s) {
    return longestPalindromicSubsequenceRecursive(s, 0, s.size() - 1);
}
```

Time Complexity : *O*(*2^N* )

Space Complexity : *O*(*N*)

### Top-down

```c++
int LPSRecursive2(const string &s, int startIndex, int endIndex, vector<vector<int>> dp) {
    if (startIndex > endIndex) {
        return 0;
    }
    //只有一个字符
    if (startIndex == endIndex) {
        return 1;
    }

    if (dp[startIndex][endIndex] == -1) {
        if (s[startIndex] == s[endIndex]) {
            dp[startIndex][endIndex] = 2 + LPSRecursive2(s, startIndex + 1, endIndex - 1, dp);
        } else {
            //从头或尾跳过一个字符
            int c1 = LPSRecursive2(s, startIndex + 1, endIndex, dp);
            int c2 = LPSRecursive2(s, startIndex, endIndex - 1, dp);
            dp[startIndex][endIndex] = max(c1, c2);
        }

    }

    return dp[startIndex][endIndex];
}

int LPS2(string s) {
    vector<vector<int>> dp(s.length(), vector<int>(s.length(), -1));
    return LPSRecursive2(s, 0, s.size() - 1, dp);
}
```

Time Complexity : *O*(*N ^ 2*)

Space Complexity : *O*(*N ^ 2*)

### bottom-up

```c++
int LPS3(string s) {
    vector<vector<int>> dp(s.length(), vector<int>(s.length()));
    for (int i = 0; i < s.length(); i++) {
        dp[i][i] = 1;
    }
    //dp[i][j]表示[i,j]区间内的字符串的最长回文子序列,上三角有效
    //从下往上，从做往右处理。
    for (int startIndex = s.length() - 1; startIndex >= 0; startIndex--) {
        for (int endIndex = startIndex + 1; endIndex < s.length(); endIndex++) {
            if (s[startIndex] == s[endIndex]) {
                dp[startIndex][endIndex] = 2 + dp[startIndex + 1][endIndex - 1];
            } else {
                dp[startIndex][endIndex] = max(dp[startIndex + 1][endIndex], dp[startIndex][endIndex - 1]);
            }
        }
    }

    return dp[0][s.length() - 1];
}
```

Time Complexity : *O*(*N ^ 2*)

Space Complexity : *O*(*N ^ 2*)

### bottom-up 优化

```c++
int lps(string s)
{
    int n = s.length();

    // dp[i] is going to store length of longest 
    // palindromic subsequence of substring s[0..i] 
    int dp[n];

    // Pick starting point 
    for (int i = n - 1; i >= 0; i--) {

        int back_up = 0;
        
        // Pick ending points and see if s[i] 
        // increases length of longest common 
        // subsequence ending with s[j]. 
        for (int j = i; j < n; j++) {

            // similar to 2D array L[i][j] == 1 
            // i.e., handling substrings of length 
            // one. 
            if (j == i)
                dp[j] = 1;

                // Similar to 2D array L[i][j] = L[i+1][j-1]+2 
                // i.e., handling case when corner characters 
                // are same.  
            else if (s[i] == s[j]){

                // value a[j] is depend upon previous  
                // unupdated value of a[j-1] but in  
                // previous loop value of a[j-1] is  
                // changed. To store the unupdated  
                // value of a[j-1] back_up variable  
                // is used. 
                int temp = dp[j];
                dp[j] = back_up + 2;
                back_up = temp;
            }else{
                // similar to 2D array L[i][j] = max(L[i][j-1], 
                // a[i+1][j]) 
                back_up = dp[j];
                dp[j] = max(dp[j - 1], dp[j]);
            }
        }
    }
    return dp[n - 1];
}
```

Time Complexity : *O*(*N ^ 2*)

Space Complexity : *O*(*N*)

## 2.  longest Palindromic Substring

> 求最长回文串

```c++

```

