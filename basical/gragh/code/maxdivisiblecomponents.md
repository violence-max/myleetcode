题目描述：

![image](/basical/gragh/image/image25.png)

解决过程：

第114场双周赛t4，没做出来

- dfs

代码：

```cpp
class Solution {
public:
    int maxKDivisibleComponents(int n, vector<vector<int>>& edges, vector<int>& values, int k) {
        vector<vector<int>> g (n);
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        int ans = 0;
        function<long long(int,int)> dfs = [&](int x, int fa) -> long long {
            long long sum = values[x];
            for (auto nx : g[x]) {
                if (nx != fa)
                    sum += dfs(nx, x);
            }
            ans += sum % k == 0;
            return sum;
        };
        dfs(0, -1);
        return ans;
    }
};
```