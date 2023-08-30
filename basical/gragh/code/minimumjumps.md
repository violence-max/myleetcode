题目描述：

![image](/basical/gragh/image/image16.png)

解决过程：

没做出来

- 最短路问题一般考虑广度优先搜索，而在无上限的图中进行广度优先搜索是不可行的，因此需要确定上界

代码：

```cpp
class Solution {
public:
    int minimumJumps(vector<int>& forbidden, int a, int b, int x) {
        unordered_set<int> s (forbidden.begin(), forbidden.end());
        queue<tuple<int, int, int>> q;
        q.emplace(0, 1, 0);
        int lower = 0, upper = max(*max_element(forbidden.begin(), forbidden.end()) + a, x) + b;
        unordered_set<int> vis;
        while (!q.empty()) {
            auto [pos, dir, cnt] = q.front();
            q.pop();
            if (pos == x) return cnt;
            int npos = pos + a;
            int ndir = 1;
            if (lower <= npos && npos <= upper && !s.count(npos) && !vis.count(npos * ndir)) {
                vis.insert(npos * ndir);
                q.emplace(npos, ndir, cnt + 1);
            }
            if (dir == 1) {
                ndir = -1;
                npos = pos - b;
                if (lower <= npos && npos <= upper && !s.count(npos) && !vis.count(npos * ndir)) {
                    vis.insert(npos * ndir);
                    q.emplace(npos, ndir, cnt + 1);
                }
            }
        }
        return -1;
    }
};
```