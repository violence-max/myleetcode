题目描述：

![image](/algorithmn/dynamic_programming/image/image84.png)

解决过程：

没做出来

- 拓扑排序dp

代码：

```cpp
class Solution {
public:
    int d[50020];
    vector<int> v[50020];
    int minimumTime(int n, vector<vector<int>>& relations, vector<int>& time) {
        for (int i = 0; i < n; ++i) v[i].clear();
        for (auto& vv : relations) {
            int f = vv[0] - 1, s = vv[1] - 1;
            v[s].push_back(f);
        }
        memset(d, -1, sizeof d);
        int ans = 0;
        // dfs(i)表示修完课程i所需的最少月数
        function<int(int)> dfs = [&](int i) -> int {
            if (d[i] != -1) return d[i];

            int r = 0;
            for (auto x : v[i]) {
                // 对前置课程进行循环，得到修完前置课程的至少月数
                if (d[x] == -1) d[x] = dfs(x);
                r = max(r, d[x]);
                // cout << x << ' ' << r << endl;
            }
            r += time[i];
            // cout << i << ' ' << r << endl;
            return d[i] = r; 
        };
        for (int i = 0; i < n; ++i) {
            ans = max(ans, dfs(i));
        }
        return ans;
    }
};
```