---
title: Leetcode每日一题05：顺次数
published: 2026-07-13
pinned: false
description: 打表法。
tags: [计算机,算法,Leetcode,枚举]
category: Leetcode每日一题
draft: false
---
题目来自Leetcode:[顺次数](https://leetcode.cn/problems/sequential-digits/description/?envType=daily-question&envId=2026-07-13)

今天这题虽标的是中等题，但其实应该算简单题。我一开始思路按位数从高到低遍历，检查后一位比前一位大1是否恒成立，这样写出来时间复杂度为$O(nlogn)$，对于此题来说已经超时了。因此考虑预处理表，直接开始打表。果然最有效的往往是最朴实无华的，符合“奥卡姆剃刀”原则。

# 题目分析
题意很简单，就是返回`[low,high]`范围内的所有"顺子"。由于在题目范围里`10<=low<=high<=10^9`，其实最快就是枚举。可以[边打表边判断](#边打表边判断),也可以[预处理表格再查询](#预处理表格枚举)。

# 边打表边判断
## 算法思路
边枚举在`10<=low<=high<=10^9`范围内的所有"顺子"，边判断这个"顺子"在不在`[low,high]`中，如果在就加入答案中。最后进行排序就行。
```cpp
class Solution {
public:
    vector<int> sequentialDigits(int low, int high) {
        vector<int> ans;
        for(int i = 1; i <= 9; i++){
            int num = i;
            for(int j = i+1; j <= 9;j++){
                num = num*10+j;
                if(num >= low && num <= high)
                    ans.push_back(num);
            }
        }
        sort(ans.begin(),ans.end());
        return ans;
    }
};
```
## 复杂度分析
- 时间复杂度:$O(1)$。
- 空间复杂度:$O(1)$。

# 预处理表格枚举
## 算法思路
在题目范围内的"顺子"，可以手算出有36个，直接放在预处理表中，然后遍历该表，边遍历边查询是否在`[low,high]`中即可。
```cpp
class Solution {
public:
    vector<int> sequentialDigits(int low, int high) {
        vector<int> ans,res;
        res={12, 23, 34, 45, 56, 67, 78, 89, 123, 234, 345, 456, 567, 678, 789, 1234, 2345, 3456, 4567, 5678, 6789, 12345, 23456, 34567, 45678, 56789, 123456, 234567, 345678, 456789, 1234567, 2345678, 3456789, 12345678, 23456789, 123456789};

        for(int i = 0; i < res.size(); i++){
            if(res[i] >= low && res[i] <= high)
                ans.push_back(res[i]);
        }

        return ans;
    }
};
```
## 复杂度分析
- 时间复杂度:$O(1)$。
- 空间复杂度:$O(1)$。