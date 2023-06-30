题目描述：

![image](/algorithmn/dynamic_programming/image/image70.png)

解决过程：

第107场双周赛t3，没做出来

- 记忆化搜索+递归
- 动态规划递推

代码：

```cpp
class Solution {
public:
    int minimizeConcatenatedLength(vector<string>& words) {
        int n = words.size();
        int memo[1000][26][26];
        memset(memo, -1, sizeof memo);
        function<int(int, char, char)> dfs = [&](int i, char j, char k) -> int {
            if (i == n) {
                return 0;
            } else if (memo[i][j - 'a'][k - 'a'] != -1) {
                return memo[i][j - 'a'][k - 'a'];
            }

            char x = words[i][0], y = words[i].back();
            int length = words[i].size();
            int len1 = dfs(i + 1, j, y) - (x == k);
            int len2 = dfs(i + 1, x, k) - (y == j);
            // cout << i << ' ' << j << ' ' << k << ' ' << len1 << ' ' << len2 << endl;
            return memo[i][j - 'a'][k - 'a'] = min(len1, len2) + length;
        };
        return dfs(1, words[0][0], words[0].back()) + words[0].size();
    }
};
```