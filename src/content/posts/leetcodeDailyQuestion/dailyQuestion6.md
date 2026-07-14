---
title: Leetcode每日一题06：最大公约数相等的子序列数量
published: 2026-07-14
pinned: false
description: 难点在于如何从"关注答案"的静态思维转变到"答案如何变化"的动态思维。
tags: [计算机,算法,Leetcode,动态规划,数论]
category: Leetcode每日一题
draft: false
---
题目来自Leetcode:[最大公约数相等的子序列数量](https://leetcode.cn/problems/find-the-number-of-subsequences-with-equal-gcd/description/?envType=daily-question&envId=2026-07-14)

今天这题是个**动态规划(DP)**的题，我初看题目总是被测试案例的答案给误导：分析答案之间的规律。但其实这题并不需要关注答案，而是关注答案是如何变化的，找到变化的规律就是解决DP问题的关键。

# 题目分析
此题简而言之就是从`nums`找出所有两个"不相交"的子序列`(seq1.seq2)`，满足`seq1`的最大公约数(GCD)等于`seq2`的GCD，返回这样的子序列对数。如果真去枚举所有的子序列对，包是想不出来的，思路上应该先考虑GCD如何变化，因为要满足GCD相等。因此考虑当`nums`元素一个一个考虑进来时，GCD会如何变化，我们会发现`gcd(a,b)=gcd(gcd(a),b)`，这样思路就有了；于是发现从`nums`添加新元素`nums[i]`，有三个去向：
- 去`seq1`，即`seq1=seq1+nums[i]`。
- 去`seq2`，即`seq2=seq2+nums[i]`。
- 直接抛弃。

这样就可以构建状态方程了，因此核心思路就是[动态规划(DP)](#动态规划dp)。

# 动态规划(DP)
## 算法思路
用 $n$ 表示数组 $nums$ 的长度，$m$ 表示数组 $nums$ 中元素的最大值。对于任意两个正整数 $a$ ，$b$ 的最大公约数 $gcd(a,b)$ ,一定满足 $gcd(a,b)\le min(a,b)$ ,其中 $min(a,b)$ 表示 $a$ , $b$ 中的最小值。因此，在任意时刻，两个子序列的 $GCD$ 都不会超过数组中的最大值 $m$。

设 $dp[i][j][k]$ 表示考虑数组中前 $i$ 个元素时，第一个子序列 $seq1$ 当前的 $GCD$ 为 $j$ ,第二个子序列 $seq2$ 当前的 $GCD$ 为 $k$ 的方案数。初始时，未考虑任何元素，两个子序列均为空，其 $GCD$ 视为 0,因此 $dp[0][0][0] = 1$。

当考虑第 $i$ 个元素 $nums[i]$ 时(下标从1开始)，对于当前状态 $dp[i-1][j][k]$,我们有三种互斥且完备的选择:
- **将 $\mathbf{nums[i]}$ 加入第一个子序列 $\mathbf{seq1}$**:此时 $seq1$ 的最大公约数变为 $gcd(j,nums[i])$ , $seq2$ 的 $GCD$ 保持 $k$ 不变。该选择为状态 $dp[i][gcd(j,nums[i])][k]$ 贡献 $dp[i-1][j][k]$ 种方案。 
- **将 $\mathbf{nums[i]}$ 加入第二个子序列 $\mathbf{seq2}$**:此时 $seq2$ 的最大公约数变为 $gcd(k,nums[i])$ , $seq1$ 的 $GCD$ 保持 $j$ 不变。该选择为状态 $dp[i][j][gcd(k,nums[i])]$ 贡献 $dp[i-1][j][k]$ 种方案。 
- **$\mathbf{nums[i]}$ 既不加入 $\mathbf{seq1}$ ,也不加入 $\mathbf{seq2}$**:两个子序列的 $GCD$ 均保持不变。该选择为状态 $dp[i][j][k]$ 贡献 $dp[i−1][j][k]$ 种方案。

这三种选择互斥且覆盖了当前元素的所有可能去向，且同一元素不能同时放入两个子序列。由此，我们可以得到递推公式：
$$
    dp[i][j][k] = dp[i-1][j][k] + \sum_{j'}dp[i-1][j'][k] + \sum_{k'}dp[i-1][j][k']  \\
    (其中j'=gcd(j,nums[i]),k'=gcd(k,nums[i]))
$$
或者等价地，在遍历时从 $dp[i−1][j][k]$ 向三个目标状态进行累加：
$$
\begin{array}{l} 
  \left\{\begin{matrix} 
   dp[i][j][k] =dp[i][j][k]+dp[i-1][j][k]\\ 
   dp[i][gcd(j,nums[i])][k]=dp[i][gcd(j,nums[i])][k]+dp[i-1][j][k]\\ 
    dp[i][j][gcd(k,nums[i])]= dp[i][j][gcd(k,nums[i])]+dp[i-1][j][k]
\end{matrix}\right.    
\end{array} 
$$
题目要求对 $10^9+7$ 取模，因此在累加过程中需同时进行取模运算。当处理完所有 $n$ 个元素后，题目要求返回 $seq1$ 的 $GCD$ 等于 $seq2$ 的 $GCD$ 且大于0的方案数(空子序列的 $GCD$ 视为0，不计入答案)。因此最终答案为:
$$
    ans = \sum_{i=1}^{m}dp[n][i][i]
$$
对上述结果取模后返回即可。

观察状态转移方程，$dp[i][⋅][⋅]$ 仅依赖于 $dp[i−1][⋅][⋅]$，因此我们可以使用滚动数组将第一维优化掉。用 $dp[j][k]$ 表示当前已处理元素下的状态，对于每个新元素 $num$，新建一个临时数组 $ndp[j][k]$ 记录加入该元素后的新状态，计算完毕后将 $dp$ 替换为 $ndp$ 即可。这样空间复杂度从 $O(n·m^2)$ 降至 $O(m^2)$。
```cpp
class Solution {
    static constexpr int MOD = 1e9 + 7;
public:
    int subsequencePairCount(vector<int>& nums) {
        int m = *max_element(nums.begin(),nums.end());//最大值
        int n = nums.size();

        vector<vector<int>> dp(m+1,vector<int>(m+1));//初始状态
        dp[0][0] = 1;

        for(int num : nums){
            vector<vector<int>> ndp(m+1,vector<int>(m+1));
            for(int j = 0; j <= m; j++){
                int divisor1 = gcd(j,num);
                for(int k = 0; k <= m; k++){
                    int val = dp[j][k];
                    if(val == 0)
                        continue;
                    
                    int divisor2 = gcd(k,num);
                    ndp[j][k] = (ndp[j][k] + val) % MOD;
                    ndp[divisor1][k] = (ndp[divisor1][k] + val) % MOD;
                    ndp[j][divisor2] = (ndp[j][divisor2] + val) % MOD;
                }
            }
            dp.swap(ndp);//状态转移
        }

        int ans = 0;
        for(int j = 1; j <= m; j++){
            ans = (ans + dp[j][j]) % MOD;
        }

        return ans;
    }
};
```
## 复杂度分析
- 时间复杂度: $O(nm^2logm)$,其中 $n$ 是数组 $nums$ 的长度，$m$ 是数组 $nums$ 中的最大元素。遍历数组 $nums$ 得到最大元素的时间是 $O(n)$,动态规划的状态数是 $O(nm^2)$,每个状态需要计算 $GCD$,需要的时间是 $O(logm)$,因此时间复杂度是 $O(nm^2logm)$。
- 空间复杂度: $O(m^2)$,其中 $m$ 是数组 $nums$ 中的最大元素。使用滚动数组优化后仅需维护两个大小为 $O(m^2)$ 的二维数组。