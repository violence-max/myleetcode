题目描述：

![image](/basical/gragh/image/image11.png)

解决过程：

写了四十多分钟写出来了

- 深度优先搜索：
    1. 建立双邻接无向图
    2. 从节点1开始深度优先搜索：
        1. 将遍历到的节点node加入到visited数组中（可以是集合可以是数组）
        2. 探讨接下来有多少个可以到达的节点数sum
        3. 如果剩余计数cnt或者sum为0，那么探讨答案的可能
        4. 遍历节点node的邻接数组，继续dfs

代码：（深度优先搜索）

```cpp
class Solution {
public:
    double frogPosition(int n, vector<vector<int>>& edges, int t, int target) {
        vector<unordered_set<int>> edges_ (n);
        for (auto edge : edges) {
            int vertext1 = edge[0] - 1, vertext2 = edge[1] - 1;
            if (!edges_[vertext1].count(vertext2)) {
                edges_[vertext1].insert(vertext2);
            }
            if (!edges_[vertext2].count(vertext1)) {
                edges_[vertext2].insert(vertext1);
            }
        }
        unordered_set<int> visited;
        double res = 0.0;

        function<void(int, int, double)> dfs = [&](int node, int cnt, double p) {
            visited.insert(node);
            int sum = 0;
            for (auto next : edges_[node]) {
                if (!visited.count(next)) {
                    sum++;
                }
            }
            if (cnt == 0 || sum == 0) {
                if (node == target - 1) {
                    res += p;
                }
                return;
            }

            for (auto next : edges_[node]) {
                if (!visited.count(next)) {
                    dfs(next, cnt - 1, p / sum);
                }
            }

        };
        dfs(0, t, 1.0);

        return res;
    }
};
```