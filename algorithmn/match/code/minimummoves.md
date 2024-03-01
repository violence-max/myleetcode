题目描述：

![image](/algorithmn/match/image/image1.png)

解决过程：

第362场周赛t3，没做出来

- 全排列匹配

代码：

```cpp
class Solution {
public:
    int minimumMoves(vector<vector<int>>& grid) {
        int n = grid.size();
        vector<pair<int,int>> from, to;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j]) {
                    for (int x = 0; x < grid[i][j] - 1; ++x) {
                        from.push_back({i,j});
                    }
                } else {
                    to.push_back({i,j});
                }
            }
        }
        int ans = INT_MAX, m = from.size();
        do {
            int s = 0;
            for (int i = 0; i < m; ++i) {
                s += abs(from[i].first - to[i].first) + abs(from[i].second - to[i].second);
            }
            ans = min(ans, s);
        } while (next_permutation(from.begin(), from.end()));
        return ans;
    }
};
```