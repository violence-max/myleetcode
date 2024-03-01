题目描述：

![image](/basicaldatastructure/binary_tree/image/image71.png)

解决过程：

没做出来，想到了递归，但是具体逻辑没思考好，尤其是在考虑子问题的时候。

代码：

```cpp
class Solution {
public:
    long long maximumScoreAfterOperations(vector<vector<int>>& edges, vector<int>& values) {
        int n = edges.size() + 1;
        vector<vector<int>> g (n);
        g[0].push_back(-1); // 防止根节点被误认为是叶子节点
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }

        // 在获取所有节点值且保证以0为根节点的树健康的情况下，失去的节点值的最小和
        // 以x为根节点的树为例子：
        // 1. 选择失去x节点的值，那么x的子树的节点值之和就可以全部加进答案中
        // 2. 选择将x节点的值加入答案中，那么变成子问题dfs(y, x)
        function<long long(int, int)> dfs = [&](int n, int fa) -> long long {
            if (g[n].size() == 1) { // 叶子节点
                // 由于第一种情况不会向下递归，因此到达根节点肯定来自于第二种情况
                // 为了保证树的健康，叶子节点必须失去，因此返回要失去的叶子节点的值
                return values[n];
            }

            long long loss = 0; // 将当前节点的值加入答案中
            for (auto nx : g[n]) {
                if (nx == fa) continue;

                loss += dfs(nx, n);
            }
            return min((long long)values[n], loss);
        };
        return accumulate(values.begin(), values.end(), 0LL) - dfs(0, -1);
    }
};
```