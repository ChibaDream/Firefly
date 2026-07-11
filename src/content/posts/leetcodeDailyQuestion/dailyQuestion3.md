---
title: Leetcode每日一题03：统计完全连通分量的数量
published: 2026-07-11
pinned: false
description: 关键在于利用完全连通分量的性质进行优先搜索。
tags: [计算机,算法,Leetcode,图,深度优先搜索(DFS),广度优先搜索(BFS)]
category: Leetcode每日一题
draft: false
---
题目来自Leetcode:[统计完全连通分量的数量](https://leetcode.cn/problems/count-the-number-of-complete-components/description/?envType=daily-question&envId=2026-07-11)

这题主要考察对图以及图论搜索算法的掌握，我愿称之为最不饶弯子的题，一起来看看吧。

# 题目分析
由题目分析可得：
- 输入**无向图**的顶点数`n`，以及边数组`edges`（其中`edges[i]=[a_i,b_i]`表示顶点`a_i`和`b_i`之间存在一条无向边）。
- 输出图中**完全连通分量**的数量。

假设一个完全连通分量有$E$条边,$V$个点,则其具有如下性质:
$$
E=\frac{V(V-1)}{2}
$$
由此性质就可自然而然地联想到**边搜索边记录连通分量的边和点数量，最后再判断是否满足该性质**即可。因此，总的核心思路便是[深度优先搜索(DFS)](#DFS)和[广度优先搜索(BFS)](#BFS)

<a id="DFS"></a>
# 深度优先搜索(DFS)
## 算法思路

首先根据输入的顶点`n`以及边`edges`进行建图，为便于搜索，采用邻接表进行存储；然后使用临时变量`V`和`E`来存储当前连通分量的顶点数和边数，在深度优先搜索的过程中，每遍历到一个点，便将`V`加一，并且把连接到这个点的所有边的数目添加到`E`中，由于在一个连通分量中每条边都统计了两遍，因此遍历完一个连通分量后如果有：`E=V*(V-1)`,答案加一。

```cpp
class Solution {
public:
    int countCompleteComponents(int n, vector<vector<int>>& edges) {
        //深度优先搜索(DFS)
        vector<int> visit(n,0);
        vector<vector<int>> g(n);//邻接表存储图
        for(auto edge:edges){
            int u = edge[0];
            int v = edge[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        int ans = 0,V,E;
        auto dfs = [&](this auto&& dfs, int u)->void{
            visit[u] = 1;
            V++;
            E += g[u].size();
            for(int v:g[u]){
                if(!visit[v]){
                    dfs(v);
                }
            }
        };

        for(int i = 0; i < n; i++){
            if(!visit[i]){
                V = 0;
                E = 0;
                dfs(i);
                ans += E == V*(V-1); //每个节点的边都遍历过一遍，相当于边数变为两倍
            }
        }
        return ans;
    }
};
```

## 复杂度分析
- 时间复杂度:$O(V+E)$，其中$V$为图中顶点数量，$E$为图中边的数量。
- 空间复杂度:$O(V+E)$，其中$V$为图中顶点的数量，$E$为图中边的数量。邻接表存图$O(V+E)$,点访问数组$visit$为$O(V)$,$dfs$递归栈深度最坏情况下为$O(V)$。

<a id="BFS"></a>
# 广度优先搜索(BFS)
## 算法思路
具体思路跟DFS无异，题目虽对`n`限制`1 <= n <= 50`，但一旦`n`变得很大，在使用DFS时会导致栈溢出。因此，有必要提一嘴BFS。

```cpp
class Solution {
public:
    int countCompleteComponents(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g(n);
        vector<int> vis(n, 0);
        int ans = 0, V, E;
        for (auto edge : edges) {
            int u = edge[0];
            int v = edge[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        auto bfs = [&](int s) -> bool {
            queue<int> q;
            q.push(s);
            while (!q.empty()) {
                int u = q.front();
                q.pop();
                if (vis[u]) {
                    continue;
                }
                vis[u] = 1;
                V++;
                E += g[u].size();
                for (auto v : g[u]) {
                    q.push(v);
                }
            }
            return E == V * (V - 1);
        };
        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                V = 0;
                E = 0;
                if (bfs(i)) {
                    ans++;
                }
            }
        }
        return ans;
    }
};
```
## 复杂度分析
- 时间复杂度:$O(V+E)$，其中$V$为图中顶点数量，$E$为图中边的数量。
- 空间复杂度:$O(V+E)$，其中$V$为图中顶点的数量，$E$为图中边的数量。邻接表存图$O(V+E)$,点访问数组$visit$为$O(V)$,$bfs$队列最坏情况下为$O(V)$。