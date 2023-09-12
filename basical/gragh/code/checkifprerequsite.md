题目描述：

![image](/basical/gragh/image/image20.png)

解决过程：

没做出来

- 拓扑排序+离线查询

代码：

```cpp
class Solution {
public:
    vector<bool> checkIfPrerequisite(int numCourses, vector<vector<int>>& prerequisites, vector<vector<int>>& queries) {
        vector<int> g[numCourses];
        bool pre[numCourses][numCourses];
        memset(pre, 0, sizeof pre);
        int degree[numCourses];
        memset(degree, 0, sizeof degree);
        for (auto& v : prerequisites) {
            ++degree[v[1]];
            g[v[0]].push_back(v[1]);
        }
        queue<int> q;
        for (int i = 0; i < numCourses; ++i) {
            if (degree[i] == 0) {
                q.push(i);
                // cout << i << endl;
            }
        }
        while (!q.empty()) {
            int cur = q.front();
            q.pop();
            for (auto& nex : g[cur]) {
                pre[cur][nex] = true;
                for (int i = 0; i < numCourses; ++i) {
                    pre[i][nex] = pre[i][nex] | pre[i][cur];
                }
                --degree[nex];
                if (degree[nex] == 0) {
                    q.push(nex);
                }
            }
        }
        int n = queries.size();
        vector<bool> ans (n);
        for (int i = 0; i < n; ++i) {
            ans[i] = pre[queries[i][0]][queries[i][1]];
        }
        return ans;
    }
};
```