题目描述：

![image](/basicaldatastructure/stackandquere/image/image34.png)

解决过程：

第372场周赛t4，没做出来

代码：

```cpp
class Solution {
public:
    vector<int> leftmostBuildingQueries(vector<int>& heights, vector<vector<int>>& queries) {
        int n = queries.size(), m = heights.size();
        vector<int> ans (n, -1);
        vector<vector<pair<int,int>>> left (m);
        for (int i = 0; i < n; ++i) {
            int u = queries[i][0], v = queries[i][1];
            if (u > v) {
                swap(u, v);
            }
            if (u == v || heights[u] < heights[v]) {
                ans[i] = v;
            } else {
                left[v].push_back({heights[u], i});
            }
        }

        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> p;
        for (int i = 0; i < m; ++i) {
            while (!p.empty() && p.top().first < heights[i]) {
                ans[p.top().second] = i;
                p.pop();
            }
            for (auto& pp : left[i]) {
                p.push(pp);
            }
        }
        return ans;
    }
};
```