题目描述：

![image](/basical/gragh/image/image24.png)

解决过程：

第364场周赛t4，没做出来

- 埃式筛+枚举质数

代码：

```cpp
const int MX = 100000;
bool np[MX+1];
int init = []() {
    for (int i = 2; i * i <= MX; ++i) {
        if (!np[i]) {
            for (int j = i * i; j <= MX; j += i) {
                np[j] = true;
            }
        }
    }
    np[1] = true;
    return 0;
}();

class Solution {
public:
    long long countPaths(int n, vector<vector<int>>& edges) {
        vector<vector<int>> g (n+1);    
        for (auto& edge : edges) {
            int u = edge[0], v = edge[1];
            g[u].push_back(v);
            g[v].push_back(u);
        }
        vector<int> size (n + 1);
        vector<int> nodes;
        function<void(int,int)> dfs = [&](int x, int fa) {
            nodes.push_back(x);
            for (auto nx : g[x]) {
                if (nx == fa || !np[nx]) continue;

                dfs(nx, x);
            }
        };
        long long ans = 0;
        for (int i = 1; i <= n; ++i) {
            if (np[i]) continue; // 类似贡献法，寻找每个质数
            long long sum = 0;
            // cout << i << endl;
            for (auto y : g[i]) {
                if (!np[y]) continue;
                // cout << i << ' ' << y << endl;
                if (size[y] == 0) {
                    nodes.clear();
                    dfs(y, -1);
                    for (auto x : nodes) {
                        size[x] = nodes.size();
                        // cout << x << ' ' << nodes.size() << endl;
                    }
                }
                ans += (long long)size[y] * sum;
                sum += size[y];
            }
            ans += sum;
        } 
        return ans;
    }
};
```