题目描述：

![image](/basical/gragh/image/image22.png)

解决过程：

没做出来

- 有向图判环

```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        // 若起始节点位于一个环内或者能够到达一个环，则节点不是安全的，否则节点是安全的
        int n = graph.size();
        vector<int> color(n);
        function<bool(int)> safe = [&](int x) -> bool {
            if (color[x] > 0) {
                return color[x] == 2;
            }
            color[x] = 1;
            for (auto xx : graph[x]) {
                if (!safe(xx)) {
                    return false;
                }
            }
            color[x] = 2;
            return true;
        };
        vector<int> ans;
        for (int i = 0; i < n; ++i) {
            if (safe(i)) 
                ans.push_back(i);
        }
        return ans;
    }
};
```