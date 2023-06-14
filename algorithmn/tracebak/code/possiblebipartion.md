题目描述：

![image](/algorithmn/tracebak/image/image28.png)

解决过程：

做了四五十分钟，没做出来

- 染色法

代码：

```cpp
class Solution {
public:
    bool possibleBipartition(int n, vector<vector<int>>& dislikes) {
        vector<int> color (n + 1);
        vector<vector<int>> adj (n + 1);
        for (const auto& dislike : dislikes) {
            int node1 = dislike[0], node2 = dislike[1];
            adj[node1].push_back(node2);
            adj[node2].push_back(node1);
        }
        for (int i = 1; i <= n; ++i) {
            // 问题转化为能否用两种颜色染遍整个图，使得不相容的节点的颜色互不相同

            if (!color[i] && !dfs(color, adj, 1, i)) {
                // 起初颜色默认染成1

                return false;
            }
        }

        return true; 
    }

    bool dfs(vector<int>& color, const vector<vector<int>>& adj, int newcolor, int node) {
        color[node] = newcolor;
        for (const auto& next : adj[node]) {
            if (color[next] && color[next] == color[node]) {
                // 不相容的节点已经被染色而且颜色和node相同

                return false;
            }
            if (!color[next] && !dfs(color, adj, 3 ^ newcolor, next)) {
                // 不相容的节点染成另外的颜色，并且深度优先搜索判断是否成功

                return false;
            }
        }
        return true;
    }
};
```