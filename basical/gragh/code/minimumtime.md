题目描述：

![image](/basical/gragh/image/image28.png)

解决过程：

没有做出来，迪杰斯特拉（堆版本）

代码：

```cpp
class Solution {
public:
    vector<int> minimumTime(int n, vector<vector<int>>& edges, vector<int>& disappear) {
        vector<vector<pair<int,int>>> gragh (n);
        for (const auto& edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            gragh[u].push_back({v, w});
            gragh[v].push_back({u, w});
        }
        priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq; // {t, u} t代表花销， u代表节点
        pq.push({0,0});
        vector<int> answer (n , -1);
        answer[0] = 0;
        while (!pq.empty()) {
            auto [t, u] = pq.top();
            pq.pop();
            if (t > answer[u]) {
                // 松弛的过程中一个节点可能重入，此时answer的值会变小
                continue;
            }
            for (const auto& [v, w]: gragh[u]) {
                if (t + w < disappear[v] && (answer[v] == -1 || t + w < answer[v])) {
                    answer[v] = t + w;
                    pq.push({t + w, v});
                }
            }
        }
        return answer;
    }
};
```