---
title: Leetcode每日一题08：数对的最大公约数之和
published: 2026-07-16
pinned: false
description: 阅读理解题。
tags: [计算机,算法,Leetcode,数论,模拟,双指针]
category: Leetcode每日一题
draft: false
---
题目来自Leetcode:[数对的最大公约数之和](https://leetcode.cn/problems/sum-of-gcd-of-formed-pairs/description/?envType=daily-question&envId=2026-07-16)

今天这题其实算简单题，题目甚至把模拟的步骤告诉了，按步骤来就行。

# 题目分析
先构造一个数组`prefixGcd`满足`prefixGcd[i] = gcd(nums[i],mx_i)`（其中`mx_i=max(nums[0],nums[1],...,nums[i])`）。然后将该数组升序排序，取排序后的首元素与末元素形成数对，求所有数对的最大公约数之和。构造数组没什么好说的，直接遍历即可；求数对最大公约数之和由于是取首尾两元素，因此可采用双指针的方法。

# 模拟
由于`mx_i`是前缀和的最大值，故可以动态构建。首尾元素遍历直接双指针即可。
```cpp
class Solution {
public:
    long long gcdSum(vector<int>& nums) {
        //构造prefixGcd数组
        vector<int> prefixGcd(nums.size());
        int mx = nums[0];
        for(int i = 0; i < nums.size(); i++){
            mx = max(mx,nums[i]);
            prefixGcd[i] = gcd(nums[i],mx);
        }
        sort(prefixGcd.begin(),prefixGcd.end());
        long long res = 0;
        //双指针
        int left=0,right=nums.size()-1;
        while(left < right){
            res += gcd(prefixGcd[left],prefixGcd[right]);
            left++;
            right--;
        }
        return res;
    }
};
```
# 复杂度分析
- 时间复杂度:$O(nlogn+nlogU)$，其中 $n$ 为 $nums$ 的长度,$U$ 为数组 $nums$ 的最大值。$O(nlogn)$ 的时间复杂度来源于排序。此外，每次计算最大公约数的时间复杂度为 $O(logU)$。
- 空间复杂度:$O(n)$。