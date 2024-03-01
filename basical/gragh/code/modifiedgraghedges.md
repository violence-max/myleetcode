题目描述：

![image](/basical/gragh/image/image12.png)

解决过程：

没做出来

代码：（二分查找+修改边权的dijkstra）

```cpp
class Solution {
public:
    vector<vector<int>> modifiedGraphEdges(int n, vector<vector<int>>& edges, int source, int destination, int target) {
        int k = 0;
        for (const auto& edge : edges) {
            if (edge[2] == -1) {
                ++k; // 统计需要修改的边的数目
            }
        }

        // 按照规则将边权为负的边修改权值过程的每一步，一定会有一条边权值加1，除了第一步直接将-1修改为1以外。
        // 将-1修改为1可以使得s到d具有可以取到的最短路，将-1修改为target可以使得s到d具有经过这些负权边可以取到的最短路的最大值，
        // 否则不会经过这些负权边（如果有其它路可以到达）。
        // 因此可以先判断两个临界条件，如果确定存在答案了之后可以使用二分法，逐一缩小答案范围。

        if (dijkstra(n, construct(n, edges, 0, target), source, destination) > target) {
            return {};  // 最短路的最小值大于target
        }
        if (dijkstra(n, construct(n, edges, static_cast<long long>(k) * (target - 1), target), source, destination) < target) {
            return {}; // 最短路的最大值小于target
        }
        long long left = 0, right = static_cast<long long>(k) * (target - 1);
        long long ans;
        while (left <= right) {
            long long mid = (left + right) >> 1;
            if (dijkstra(n, construct(n, edges, mid, target), source, destination) >= target) {
                ans = mid;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }

        for (auto& edge : edges) {
            if (edge[2] == -1) {
                if (ans >= target - 1) {
                    edge[2] = target;
                    ans -= (target - 1);
                } else {
                    edge[2] = ans + 1;
                    ans = 0;
                }
            }
        }

        return edges;
    }

    long long dijkstra(int n, const vector<vector<int>>& adj, int source, int destination) {
        vector<long long> dist (n, INT_MAX / 2);
        vector<bool> visited (n, false);
        dist[source] = 0;

        for (int i = 0; i < n; ++i) {
            int u = -1;
            for (int j = 0; j < n; ++j) {
                if (!visited[j] && (u == - 1 || dist[j] < dist[u])) {
                    u = j;
                }
            }
            visited[u] = true;

            for (int k = 0; k < n; ++k) {
                // 松弛
                if (adj[u][k] != INT_MAX) {
                    dist[k] = min(dist[k], dist[u] + adj[u][k]);
                }
            }
        }

        return dist[destination];
    }

    vector<vector<int>> construct(int n, const vector<vector<int>>& edges, long long idx, int target) {
        // 将带有负权（-1）的边集按规则修改为正权的邻接图
        // 规则如下：

        /*
        假设有k条边权重为-1，则要将其权重修改为范围为[1,2*1e9]中的正整数，最小的情况是全1。但是若将边修改为大于target的数是没有
        意义的，因为如果这样最终的最短路径不会经过该条边。因此每条边可以修改的范围应该是[1,target]。
        修改示意图如下：
        [1,1,1,1,……,1,1,1,1]: idx = 0
        [2,1,1,1,……,1,1,1,1]: idx = 1
        [3,1,1,1,……,1,1,1,1]: idx = 2
        ……
        [target,1,1,1,……,1,1,1,1]: idx = target - 1
        [target,2,1,1,……,1,1,1,1]: idx = target
        ……
        [target,target,target,target,……,target,target,target,target]: idx = k * (target - 1)

        在已经知道s-t最短路长度为dmin的情况下，如果将一条边的长度加1，要么dmin不变，要么dmin加1。因此我们可以无视排列组合，一直
        将一条边加1知道所有边都加满为止，便可以试遍所有可行性，至少一定能找到一组解。
        */

        vector<vector<int>> adj (n, vector<int> (n, INT_MAX));
        for (const auto& edge : edges) {
            int u = edge[0], v = edge[1], w = edge[2];
            if (w != -1) { // 边权为正直接赋值
                adj[u][v] = adj[v][u] = w;
            } else {
                if (idx >= target - 1) { // 需要找规律
                    adj[u][v] = adj[v][u] = target;
                    idx -= (target - 1);
                } else {
                    adj[u][v] = adj[v][u] = idx + 1;
                    idx = 0;
                }
            }
        }

        return adj;
    }
};
```