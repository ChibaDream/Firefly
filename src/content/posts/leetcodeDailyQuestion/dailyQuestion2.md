---
title: Leetcode每日一题02：针对图的路径存在性查询Ⅱ
published: 2026-07-10
pinned: false
description: 难点在于使用倍增法优化最短路径计算时间
tags: [计算机,算法,Leetcode,贪心,双指针,动态规划,倍增法]
category: Leetcode每日一题
draft: false
---
题目来自Leetcode:[针对图的路径存在性查询Ⅱ](https://leetcode.cn/problems/path-existence-queries-in-a-graph-ii/description/)

这题的前置题为[针对图的路径存在性查询Ⅰ](/posts/leetcodedailyquestion/dailyquestion1/),前置题和这道题比起来简直就是个萝莉！这题的数组一旦无序，难度是暴增的，而且看题目以为是图论题，想半天都不知道怎么优化，但实际上就不是图论题。参考了这个大神的[题解](https://leetcode.cn/problems/path-existence-queries-in-a-graph-ii/solutions/3663266/pai-xu-shuang-zhi-zhen-bei-zeng-pythonja-ckht/)，我才逐渐明白此题的核心思路。

# 题目分析
首先，和上道题不同的是，`nums`不再是有序的，并且要求返回结果是节点`u_i`和节点`v_i`之间的最短距离。由于`nums`无序，因此如果用图算法，例如DFS或BFS来做的话，时间复杂度$O(n^2)$是会超时的。因此需要换个思路，图的边是由条件`|nums[i]-nums[j]| <= maxDiff`决定的，那其实可以变成一维数轴上点的最少步数问题：将节点按数值升序排序，假设查询`[u,v](u<v)`,`v`向`u`跳，每一步的距离不超过`maxDiff`；那么跳一步，最多可以跳多远？并且最少要跳多少步？

对于“最多可以跳多远？”这个问题，很容易想到双指针，因为已经排过序了，这样就可以找到节点`i`向左最多跳到哪个位置;“最少要跳多少步？”这个问题可以一步一步跳，但时间容易超时，因此pass,但有一个想法是很自然的：尽量贪心地向左跳，因此改用倍增法来进行跳步优化，时间由$O(n)$变到$O(logn)$。

由上，本题思路为[双指针+倍增法](#Dpointer-DP)。为了更好地理解难点**倍增法**的思想，请先简短地看下视频
<iframe width="100%" height="468" src="//player.bilibili.com/player.html?bvid=BV1T7iEBnEcx&p=1&autoplay=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" &autoplay=0> </iframe>

# 倍增法(Binary Lifting)
## 基本概念
倍增法，顾名思义就是成倍增长。我们在进行递推时，如果状态空间很大，通常的线性递推无法满足时间与空间复杂度的要求，那么我们可以通过成倍增长的方式，只递推状态空间中在$k$的整数次幂位置上的值作为代表。当需要其他位置上的值时，我们通过"任意整数可以表示成若干个$k$的次幂项的和"这一性质，使用之前求出的代表值拼成所需的值。所以使用倍增算法也要求我们递推的问题的状态空间关于$k$的次幂具有可划分性．通常情况下$k$取2。
## 思想实现
由视频和概念可总结出，由于整数都可以转换成二进制表示，因此跳的步数可以变成以$2^k$步跳，以此来优化最短路径的查询时间。因此可以预处理一个倍增表`jump[i][k]`表示从`i`出发，连续跳$2^k$步后能到达的最左位置,`jump[i][0]`已由双指针找到，之后的`jump[i][k]`可由以下性质递推计算得出
$$
    2^k=2^{k-1}+2^{k-1}
$$
由此，`jump[i][k]=jump[jump[i][k-1]][k-1]`，从结构可看出，倍增其思想本质来自**动态规划(DP)**。

<a id="Dpointer-DP"></a>
# 双指针+倍增法
## 思路
本题的`nums`不是有序的，我们需要做一个映射，把询问的节点编号映射成一个有序数组的下标，这样就方便处理了。
- 创建一个下标数组`idx=[0,1,2,...,n-1]`,然后按照`nums[idx[i]]`从小到大排序。
- 定义`rank[i]`表示节点`i`在`idx`中的下标，即节点`i`映射为编号`rank[i]`。

排完序后，就可利用双指针预处理倍增表`pa[i][0]`的数据，其余数据则由动态规划递推得出，最终构建出完整的`pa[i][k]`。

**关键思路**：每一步都贪心地往左跳(即从2的高次幂到低次幂跳)。跳的每$2^k$步，如果`r<=l`，说明越过了目标位置，则不跳；反之则跳。如果跳完的位置还是`l<r`，说明找不到，返回-1，反之则答案加1。
```cpp
class Solution {
public:
    vector<int> pathExistenceQueries(int n, vector<int>& nums, int maxDiff, vector<vector<int>>& queries) {
        vector<int> idx(n);
        ranges::iota(idx,0);//生成从0开始的递增序列到idx中
        ranges::sort(idx,{},[&](int i){ return nums[i];});//将idx数组根据nums[idx[i]]从小到大排序

        //rank[i]表示节点i在idx中的下标
        vector<int> rank(n);
        for(int i = 0; i < n; i++){
            rank[idx[i]] = i;
        }

        //双指针
        int mx = bit_width((uint32_t) n);//计算n二进制表示的位数
        vector pa(n,vector<int>(mx));//创建倍增预处理表
        int left = 0;
        for(int i = 0; i < n; i++){
            while(nums[idx[i]] - nums[idx[left]] > maxDiff){
                left++;
            }
            pa[i][0] = left;
        }

        //倍增
        for(int i = 0; i < mx-1; i++){
            for(int x = 0; x < n; x++){
                int p = pa[x][i];
                pa[x][i+1] = pa[p][i];
            }
        }

        //倍增查询
        vector<int> ans(queries.size());
        for(int qi = 0; qi < queries.size(); qi++){
            int l = queries[qi][0], r = queries[qi][1];
            if(l == r){
                continue;//不用跳
            }
            l = rank[l];
            r = rank[r];
            if(l > r){
                swap(l,r);
            }
            //从r开始,向左跳到l
            int res = 0;
            for(int k = mx-1; k >= 0; k--){
                if(pa[r][k] > l){
                    res |= 1 << k;//累加步数
                    r = pa[r][k];//下个位置
                }
            }
            ans[qi] = pa[r][0] > l ? -1 : res+1;//再跳一步就能到l
        }
        return ans;
    }
};
```
## 复杂度分析
- 时间复杂度:$O((n+q)logn)$，其中$n$是$nums$的长度,$q$是$queries$的长度。
- 空间复杂度:$O(nlogn)$。返回值不计入。

# 细节说明
## 为什么不从左到右跳呢？
其实从左到右跳与从右向左跳都可以，因为是无向边，但题解设为从右往左跳主要想提示此题跟**树节点的第K个祖先**相关，因为这个数轴可看成树，数值大的节点在下边，最短路径就是找到祖先的路径。
## 为什么向左倍增跳的过程排除刚好跳到目标位置的步数？
这个问题实质上是在问将代码中的
```cpp
for(int k = mx-1; k >= 0; k--){
                if(pa[r][k] > l){
                    res |= 1 << k;//累加步数
                    r = pa[r][k];//下个位置
                }
            }
```
改为
```cpp
for(int k = mx-1; k >= 0; k--){
                if(pa[r][k] >= l){
                    res |= 1 << k;//累加步数
                    r = pa[r][k];//下个位置
                }
            }
```
首先，因为标准的倍增过程是逼近的过程，不处理到达的逻辑，除非你新加等于的逻辑，但这样又要处理等于的逻辑，很没必要；其次，因为倍增过程要循环$k$次，如果提前到达目标位置，还要接着循环，容易导致重复计算。例如`pa[i][1]=pa[i][0]=0`，在跳到目标位置即左端点，下个位置还是自身，由于循环还没完成，这样条件又会成立，答案会增加，导致错误。

因此最终编程向左跳到目标位置的右边一步，既保证了编程的统一和简洁，又保证不出错。