---
title: Leetcode每日一题07：奇数和与偶数和的最大公约数
published: 2026-07-15
pinned: false
description: 数学的重要性。
tags: [计算机,算法,Leetcode,数论]
category: Leetcode每日一题
draft: false
---

题目来自Leetcode:[奇数和与偶数和的最大公约数](https://leetcode.cn/problems/gcd-of-odd-and-even-sums/description/?envType=daily-question&envId=2026-07-15)

此题与其说是算法题，不如说是数学题，再次说明了数学的重要性。

# 题目分析
本题简而言之就是给出整数`n`，求出前`n`项奇数和与偶数和的最大公约数。前`n`项和，尤其是奇数和与偶数和都为等差数列，直接可由公式得到，最后返回两个的`GCD`即可。不过在数论里`GCD`有个性质是`gcd(ka,kb)=kgcd(a,b)`([数学证明在这](#数学证明))，由此可进一步优化。

# 等差求和法
## 算法思路
由等差数列公式可得
$$
    \begin{align*}
        sumOdd &= 1+3+...+(2n-1)=n^2\\
        sumEven &= 2+4+...+(2n)=n(n+1)
    \end{align*}
$$
结果即为`gcd(n*n,n(n+1))`。
```cpp
class Solution {
public:
    int gcdOfOddEvenSums(int n) {
        return gcd(n*n,n*(n+1));
    }
};
```
## 复杂度分析
- 时间复杂度:$O(logn)$。
- 空间复杂度:$O(logn)$,递归栈深度最坏情况会达到$O(logn)$。

# 数学
## 算法思路
由 $gcd(ka,kb)=kgcd(a,b)$ 可知
$$
gcd(n^2,n(n+1))=ngcd(n,n+1)
$$
又 $n$ 与 $n+1$ 互质,因此 $gcd(n,n+1)=1$ ,故 $gcd(n^2,n(n+1))=n$ 。
```cpp
class Solution {
public:
    int gcdOfOddEvenSums(int n) {
        return n;
    }
};
```
## 复杂度分析
- 时间复杂度:$O(1)$。
- 空间复杂度:$O(1)$。

# 数学证明
对任意整数 $k \le 0$，有
$$
    gcd(ka,kb) = k\cdot gcd(a,b)
$$

**证明**:

设 $d=gcd(a,b)$,则 $a=d\cdot a'$, $b=d\cdot b'$,且 $gcd(a',b')=1$。那么
$$
    ka=kda',kb=kdb'
$$
此时 $kd$ 是 $ka$ 和 $kb$ 的公因子,且因为 $gcd(a',b')=1$, $kd$ 就是最大公因子,即:
$$
    gcd(ka,kb)=kd=k\cdot gcd(a,b)
$$
证毕。