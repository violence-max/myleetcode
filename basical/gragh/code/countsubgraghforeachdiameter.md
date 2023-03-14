题目描述：  
![image](/basical/gragh/image/image9.png)  
解题过程：  
没做出来  
题解的三种思路：枚举子树+动态规划→bfs+dfs  
- 枚举子树加动态规划：
1. 将1~n的节点编号成0~n-1放置如邻接表数组中，编号从0开始是为了方便后续枚举子树
2. 从掩码1开始到(1 << n)结束开始枚举子树，就是常规的思路，1代表这个位置有节点而0代表没有
3. 使用动态规划法求树的直径：从一个结点开始，记最远路径为first，次远路径为second，则直径为所有节点的first + second的最大值
4. 在这个题目中的dfs函数中没有像以往计算树的直径那样记录一个parent—因为掩码的存在
5. 每遍历一个节点掩码对应的位置变更为0
- bfs+dfs：
1. 广度优先搜索判断子树的连通性，并且同时得到子树中的最远端点y
2. 如果子树连通则从y开始进行深度优先搜索求子树的直径并且记录答案  
代码：（枚举子树+动态规划→bfs+dfs） 
```cpp
class Solution {
public:
    void test(vector<vector<int>> adj) {
        for (int i = 0; i < adj.size(); i++) {
            cout << i << ' ';
            for (auto v : adj[i]) {
                cout << v << ' ';
            }
            cout << endl;
        }
    }
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj (n);
        for (auto& edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }

        function<int(int,int&,int&)> dfs = [&] (int root, int& mask, int& diameter) {
            int first = 0, second = 0;
            mask &= ~(1 << root);
            for (auto vertext : adj[root]) {
                if (mask & (1 << vertext)) {
                    int distance = 1 + dfs(vertext,mask,diameter);
                    if (distance > first) {
                        second = first;
                        first = distance;
                    } else if (distance > second) {
                        second = distance;
                    }
                }
            }
            diameter = max(diameter, first + second);
            return first;
        };

        vector<int> ans (n - 1);
        for (int i = 1; i < (1 << n); i++) {
            int x = 32 - __builtin_clz(i) - 1;
            int mask = i;
            int diameter = 0;
            dfs(x,mask,diameter);
            if (mask == 0 && diameter > 0) {
                ans[diameter-1]++;
            }
        }
        return ans;
    }
};
```  
```cpp
class Solution {
public:      
    vector<int> countSubgraphsForEachDiameter(int n, vector<vector<int>>& edges) {
        vector<vector<int>> adj(n);        
        for (auto &edge : edges) {
            int x = edge[0] - 1;
            int y = edge[1] - 1;
            adj[x].emplace_back(y);
            adj[y].emplace_back(x);
        }

        function<int(int, int, int)> dfs = [&](int parent, int u, int mask)->int {
            int depth = 0;
            for (int v : adj[u]) {
                if (v != parent && mask & (1 << v)) {
                    depth = max(depth, 1 + dfs(u, v, mask));
                }
            }
            return depth;
        };

        vector<int> ans(n - 1);
        for (int i = 1; i < (1 << n); i++) {
            int x = 32 - __builtin_clz(i) - 1;
            int mask = i;
            int y = -1;
            queue<int> qu;
            qu.emplace(x);
            mask &= ~(1 << x);
            while (!qu.empty()) {
                y = qu.front();
                qu.pop();
                for (int vertex : adj[y]) {
                    if (mask & (1 << vertex)) {
                        mask &= ~(1 << vertex);
                        qu.emplace(vertex);
                    }
                }
            }
            if (mask == 0) {
                int diameter = dfs(-1, y, i);
                if (diameter > 0) {
                    ans[diameter - 1]++;
                }
            }
        }
        return ans;
    }
};
```