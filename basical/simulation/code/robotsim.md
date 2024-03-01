题目描述：

![image](/basical/simulation/image/image6.png)

解题过程：

暴力模拟就好

代码：

```cpp
class Solution {
public:
    const int directions[4][2] = {
        {0, 1},
        {1, 0},
        {0, -1},
        {-1, 0},
    };

    int robotSim(vector<int>& commands, vector<vector<int>>& obstacles) {
        int n = commands.size();
        unordered_set<int> s;
        for (const auto& v : obstacles) {
            int x = v[0], y = v[1];
            s.insert(x * 60020 + y);
        }
        int d = 0;
        int r = 0, c = 0;
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            if (commands[i] == -1) {
                d = (d + 1) % 4;
            } else if (commands[i] == -2) {
                d = (d + 3) % 4;
            } else {
                int tmp = commands[i];
                while (tmp--) {
                    int nr = r + directions[d][0], nc = c + directions[d][1];
                    if (s.count(nr * 60020 + nc)) break;
                    r = nr;
                    c = nc;
                }
                ans = max(ans, (long long)r*r + (long long)c*c);
            }
        }
        return ans;
    }
};
```