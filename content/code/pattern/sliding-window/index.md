---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Pattern-2 Sliding Window"
subtitle: ""
summary: "滑动窗口"
authors: []
tags: ["pattern"]
categories: []
date: 2020-05-23T17:44:03+08:00
lastmod: 2020-05-23T17:44:03+08:00
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

### 1、介绍

适用于求**连续**子数组

### 2、average of subarrarys of size K 

> 求长度为K的连续子数组的平均值

```c++
input:	arrary:[1,3,2,6,-1,4,1,8,2] ,K=5
    
output:	[2.2,2.8,2.4,3.6,2,8]
```

code:

```c++
vector<double> averageOfSubarrayOfSizeK(int k,vector<int> &arr){
    vector<double> result(arr.size() -k +1);
    int windowStart =0;
    double windowSum =0;
    
    for(int windowEnd =0 ; windowEnd<arr.size() ;windowEnd++){
        //加上下一个元素
        windowSum += arr[windowEnd];
        
        //到达sizeK，滑动窗口
        if(windowEnd >=k-1){
            result[windowStart]=windowSum/k; //计算平均值
            windowSum -=arr[windowStart];  //减去第一个窗口值
            windowStart++; //滑动窗口
        }
    }
    return result;
}
```



### 2、maximum sum of subarray of size K

> 求长度为K的连续子数组的最大值
>
> 其中数组元素为正数，K为正数

```c++
input:	[2,1,5,1,3,2], k=3

output:	9
    
subarray:[5,1,3]
```

```c++
input:	[2,3,4,1,5] ,k=2

output:	7
    
 subarray:[3,4]
```

code:

```c++
int maxSumSubarrayOfSizeK(int k ,vector<int> &arr){
    int windowStart =0;
    int windowSum=0;
    int maxSum=0;
    
    for(int windowEnd =0; windowEnd<arr.size() ;windowEnd++){
        windowSum +=arr[windowEnd];
        if (windowEnd>=k-1){
            maxSum=max(maxSum,windowSum);
            windowSum -= arr[windowStart];
            windowStart++;
        }
    }
    return maxSum;
}
```

Time:	*O*(N) 

Space:	*O*(1)



### 3、smallest subarray whose sum is greater than or equal to S

> 求最短长度的子数组，使得其和大于等于 S
>
> 其中数组元素为正数，S 为正数；如果子数组不存在，返回0

```c++
input:	[2,1,5,2,3,2], S=7
    
output:	2

subarray:[5,2]
```

```c++
input:	[3,4,1,1,6],S=8
    
output:	3
    
subarray:[3,4,1],[1,1,6]
```

code：

```c++
int findMinSubarray(int k, vector<int> &arr){
    int windowSum = 0;
    int minLength = INT_MAX;
    int windowStart = 0;
    
    for (int windowEnd = 0; windowEnd < arr.size(); windowEnd++){
        windowSum += arr[windowEnd];
        
        //当和大于等于K时候，减小窗口大小
        //不确定减去窗口第一个值后，是否满足要求，所以不断减，直到满足K
        while (windowSum >= k){
            //记录满足条件的最小窗口长度
            minLength = min(minLength, windowEnd - windowStart + 1);
            windowSum -= arr[windowStart];
            windowStart++;
        }
    }
    return minLength == INT_MAX ? 0 : minLength;
}
```

Time:	*O*(N) 

Space:	*O*(1)

### 4、longest subarrary with no more than K distinct characters

> 求包含 不超过K个不同字符 的最长子串的长度

```c++
input:	string="araaci" ,k=2

output：	4
    
substring:"araa"
```

```c++
input:	string="cbbebi" ,k=3

output：	5
    
substring:"cbbeb" "bbebi"
```

code:

```c++
int findLength(int k, const string &str) {
    int maxLength = 0;
    int windowStart = 0;
    
    //记录处理过的字符的频率
    unordered_map<char, int> mp;
    
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
        char rightchar = str[windowEnd];
        mp[rightchar]++;
        //不断减小窗口长度，直到满足K个不同字符
        while (mp.size() > k) {
            char leftchar = str[windowStart];
            mp[leftchar]--;
            //删除值为0的键
            if (mp[leftchar] == 0) {
                mp.erase(leftchar);
            }
            windowStart++;
        }
        //记录满足K的最大长度
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;
}

```

Time:	*O*(N) 

Space:	*O*(K)



### 5、maximum number of fruits in each basket

> 字符数组，每个字符代表一种果树，给2个篮子，每个篮子只能装一种水果
>
> 可以从数组任意位置装，不能回头，遇到第三种水果结束
>
> 求两个篮子能装水果的最大值

> 同4 ，K=2

code:

```c++
int findLength(int k, const string &str) {
    int maxLength = 0;
    int windowStart = 0;
    
    //记录处理过的字符的频率
    unordered_map<char, int> mp;
    
    for (int windowEnd = 0; windowEnd < str.length(); windowEnd++) {
        mp[str[windowEnd]]++;
        //不断减小窗口长度，直到满足K个不同字符
        while (mp.size() > k) {
            mp[str[windowStart]]--;
            //删除值为0的键
            if (mp[str[windowStart]] == 0) {
                mp.erase(str[windowStart]);
            }
            windowStart++;
        }
        //记录满足K的最大长度
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;
}

```



### 6、no repeat substring

> 求无重复字符的子串的最大长度

```c++
input:	string="aabccbb"

output:	3

substring:"abc"
```

```c++
input:	string="abccde"

output:	3

substring:"abc" "cde"
```

code:

```c++
int findlength(const string &str) {
    int windowStart = 0;
    int maxLength = 0;

    //记录处理后每个字符的最后索引位置，是索引位置，不是频率
    unordered_map<char, int> mp;

    for (int windowEnd = 0; windowEnd < str.size(); windowEnd++) {
        char rightChar = str[windowEnd];

        //如果mp中已经包含rightchar,从windowStart侧缩小窗口
        if (mp.find(rightChar) != mp.end()) {

            //此时窗口还未添加重复元素，改变窗口起始位，比较重复原元素后一位和windowStar大小
            windowStart = max(windowStart, mp[rightChar] + 1);

        }
        //插入rightChar
        mp[rightChar] = windowEnd;
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;

}
```

Time:	*O*(N) 

Space:	*O*(K)



### 7、longest substring with same letters after replacement

> 取代不超过K个元素，求包含最长相同元素的字串
>
> 其中字符串全是小写

```c++
input:	string="aabccbb" ,k=2

output:	5

substring: bccbb->bbbbb
```

```c++
input:	string="abccde" ,k=1

output:	3

substring: acc->ccc ccd->ccc
```

code:

```c++
int findLength(const string &str, int k) {
    int windowStart = 0;
    int maxLength = 0;
    //记录每个窗口内重复最多的元素的个数
    int maxRepeatLetterCount = 0;
    //频率
    unordered_map<char, int> frequencyMap;

    for (int windowEnd = 0; windowEnd < str.size(); windowEnd++) {
        char rightChar = str[windowEnd];
        frequencyMap[rightChar]++;
        maxRepeatLetterCount = max(maxRepeatLetterCount, frequencyMap[rightChar]);

        //窗口的长度减去最多重复的个数，剩下的是可以替换的个数，如果超过K，就要缩小窗口
        if (windowEnd - windowStart + 1 - maxRepeatLetterCount > k) {
            char leftChar = str[windowStart];
            frequencyMap[leftChar]--;
            windowStart++;
        }
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;
}
```

Time:	*O*(N) 

Space:	*O*(1)



### 8、longest subarray with ones after replacement

> 数组只包含0和1，将不超过K个0取代为1，求最长连续只包含1的子数组

> 同7

```c++
input: arrary=[0,1,1,0,0,0,1,1,0,1,1],k=2

output:	6

index=5,8; 0->1
```

```c++
input: arrary=[0,1,0,0,1,1,0,1,1,0,0,1,1] ,k=3

output:	9

index=6,9,10; 0->1
```

code:

```c++
int findLength(const vector<int> &arr, int k) {
    int windowStart = 0;
    int maxLength = 0;
    //记录每个窗口中1的个数
    int maxOnesCount = 0;

    for (int windowEnd = 0; windowEnd < arr.size(); windowEnd++) {
        if (arr[windowEnd] == 1) {
            maxOnesCount++;
        }
        if (windowEnd - windowStart + 1 - maxOnesCount > k) {
            if (arr[windowStart] == 1) {
                maxOnesCount--;
            }
            windowStart++;
        }
        maxLength = max(maxLength, windowEnd - windowStart + 1);
    }
    return maxLength;
}

```

Time:	*O*(N) 

Space:	*O*(1)



### 9、permutation in a string 

> 文本串和模式串，判断文本串中是否包含模式串的排列
>
> 串的排列：str=“abc”，排列：“abc","acb","bac","bca","cab","cba"

```c++
input:string="oidbcaf",pattern="abc"
    
output: true
    
permutation:bca<->abc
```

```c++
input:string="odicf",pattern="dc"
    
output: false
```

code:

```c++
bool findPermutation(const string &str, const string &pattern) {
    int windowStart = 0;
    int matched = 0;
    unordered_map<char, int> frequencyMap;
    for (auto c: pattern) {
        frequencyMap[c]++;
    }

    for (int windowEnd = 0; windowEnd < str.size(); windowEnd++) {
        char rightChar = str[windowEnd];
        if (frequencyMap.find(rightChar) != frequencyMap.end()) {
            frequencyMap[rightChar]--;
            if (frequencyMap[rightChar] == 0) {
                matched++;
            }

        }
        if (matched == (int) frequencyMap.size()) {
            return true;
        }

        if (windowEnd >= pattern.length() - 1) {
            char leftChar = str[windowStart];
            windowStart++;
            if (frequencyMap.find(leftChar) != frequencyMap.end()) {
                if (frequencyMap[leftChar] == 0) {
                    matched--;
                }
                frequencyMap[leftChar]++;
            }
        }
    }
    return false;
}

```







### 10、string anagrams

> 文本串和模式串，找出文本串中包含模式串的同文异构词的起始索引

> 同文异构词就是全排列

```c++
input:string="ppqp",pattern="pq"
    
output: [1,2]
    
anagram:"pq","qp",startIndex=[1,2]
```

```c++
input:string="abbcabc",pattern="abc"
    
output: [2,3,4]
    
anagram:"bca","cab","abc",startIndex=[3,4,5]
```

code：

```c++
vector<int> findStringAnagrams(const string &str, const string &pattern) {
    int windowStart = 0;
    int matched = 0;
    unordered_map<char, int> frequencyMap;
    vector<int> resultIndices;
    for (auto c: pattern) {
        frequencyMap[c]++;
    }

    for (int windowEnd = 0; windowEnd < str.size(); windowEnd++) {
        char rightChar = str[windowEnd];
        if (frequencyMap.find(rightChar) != frequencyMap.end()) {
            frequencyMap[rightChar]--;
            if (frequencyMap[rightChar] == 0) {
                matched++;
            }

        }
        if (matched == (int) frequencyMap.size()) {
            resultIndices.push_back(windowStart);
        }

        if (windowEnd >= pattern.length() - 1) {
            char leftChar = str[windowStart];
            windowStart++;
            if (frequencyMap.find(leftChar) != frequencyMap.end()) {
                if (frequencyMap[leftChar] == 0) {
                    matched--;
                }
                frequencyMap[leftChar]++;
            }
        }
    }
    return resultIndices;
}
```



### 11、smallest window containing substring

> 文本串和模式串，找出最短的文本字串，使得字串包含模式串所有元素

> 与9相似

```c++
input:string="aabdec",pattern="abc"
    
output: "abdec"
```

```c++
input:string="abdabca",pattern="abc"
    
output: "abc"
```

code:

```c++
string findSubstring(const string &str, const string &pattern) {
    int windowStart = 0;
    int matched = 0;
    int minLength = str.length() + 1;
    int subStrStart = 0;
    unordered_map<char, int> frequencyMap;

    for (auto c: pattern) {
        frequencyMap[c]++;
    }

    for (int windowEnd = 0; windowEnd < str.size(); windowEnd++) {
        char rightChar = str[windowEnd];
        if (frequencyMap.find(rightChar) != frequencyMap.end()) {
            frequencyMap[rightChar]--;
            if (frequencyMap[rightChar] == 0) {
                matched++;
            }

        }
        while (matched == (int) pattern.length()) {
            if (minLength > windowEnd - windowStart + 1) {
                minLength = windowEnd - windowStart + 1;
                subStrStart = windowStart;
            }
            char leftChar = str[windowStart];
            windowStart++;
            if (frequencyMap.find(leftChar) != frequencyMap.end()) {
                if (frequencyMap[leftChar] == 0) {
                    matched--;
                }
                frequencyMap[leftChar]++;
            }
        }
    }
    return minLength > str.length() ? "" : str.substr(subStrStart, minLength);
}
```



### 12、words concatenation

>  给定一个字符串和一组相同长度的单词组成的字符串数组，在字符串中查找包含数组所有单词的子串，单词在字串中不重叠，求子串的起始位置

```c++
input:string="catfoxcat",words=["cat","fox"]
    
output: [0,3]

substring:"catfox" -> index=0 ;"foxcat" ->index=3
```

```c++
input:string="catcatfoxfox",words=["cat","fox"]
    
output: [3]

substring:"catfox" -> index=3 
```

code:

```c++
程序有误
```

