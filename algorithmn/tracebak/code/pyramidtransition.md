题目描述：

![image](/algorithmn/tracebak/image/image31.png)

解决过程：
没做出来

要点：

1. 自底向上推导，二进制辅助
2. 8进制来防止重复判断

代码：

```cpp
class Solution {
public:
    int a[7][7];
    int n;
    bool pyramidTransition(string bottom, vector<string>& allowed) {
        n = bottom.size();
        memset(a, 0, sizeof a);
        for (const string& str : allowed) {
            a[str[0] - 'A'][str[1] - 'A'] |= 1 << (str[2] - 'A');
        }
        vector<vector<int>> b (n, vector<int> (n, 0));
        int id = 0;
        for (const auto& c : bottom) {
            b[n-1][id++] = c - 'A';
        }
        unordered_set<int> seen;
        function<bool(int, int, int)> dfs = [&](int R, int N, int i) -> bool {
            if (N == 1 && i == 1) {
                return true;
            } else if (i == N) {
                if (seen.count(R)) {
                    return false;
                }
                seen.insert(R);
                return dfs(0, N - 1, 0);
            } else {
                int w = a[b[N][i]][b[N][i+1]];
                for (int k = 0; k < 7; ++k) {
                    if (w & (1 << k)) {
                        b[N-1][i] = k;
                        if (dfs(R * 8 + (k + 1), N, i + 1)) {
                            return true;
                        }
                    }
                }
                return false;
            }
        };
        return dfs(0, n - 1, 0);
    }
};
```