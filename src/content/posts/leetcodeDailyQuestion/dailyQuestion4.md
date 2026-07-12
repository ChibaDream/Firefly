---
title: Leetcode每日一题04：数组序号转换
published: 2026-07-12
pinned: false
description: 哈希表的妙用。
tags: [计算机,算法,Leetcode,哈希表,排序]
category: Leetcode每日一题
draft: false
---
题目来自Leetcode:[数组序号转换](https://leetcode.cn/problems/rank-transform-of-an-array/description/?envType=daily-question&envId=2026-07-12)

今天这题是个简单题，经过的前天的磨练后，我现在强的可怕。话不多说，进入正题。

# 题目分析
题目大意就是将数组升序排序的原下标(从1开始)返回。很容易想到的思路是先排序，再将排序后对应的下标用哈希表存储，即`ranks[sortArr[i]]=i+1`;最后通过哈希表映射回来即可。
```cpp
class Solution {
public:
    vector<int> arrayRankTransform(vector<int>& arr) {
        vector<int> sortArr = arr;
        sort(sortArr.begin(),sortArr.end());
        unordered_map<int,int> ranks;
        vector<int> res;
        for(auto a:sortArr){
            if(!ranks.count(a)){
                ranks[a] = ranks.size()+1;
            }
        }
        for(int i = 0; i < arr.size(); i++){
            res.push_back(ranks[arr[i]]);
        }

        return res;
    }
};
```
# 复杂度分析
- 时间复杂度:$O(nlogn)$，其中$n$是输入数组$arr$的长度，排序消耗$O(nlogn)$时间。
- 空间复杂度:$O(n)$。有序数组和哈希表各消耗$O(n)$空间。
