题目描述：

![image](/algorithmn/dynamic_programming/image/image93.png)

解决过程：

第356场周赛t4，没做出来，有个小地方写错了，主要是t3花时间太久了，t4没做出来

代码：

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;
    int memo[101][10];
    int calc(const string& str) {
        int l = str.size();
        function<int(int, bool, bool, int)> dfs = [&](int i, bool isnum, bool pre, int lc) -> int {
            // cout << i << ' ' << isnum << ' ' << pre << ' ' << lc << endl;
            if (i == l) return isnum;
            else if (!pre && isnum && memo[i][lc] != -1) return memo[i][lc];

            int ret = 0;
            if (!isnum)
                ret = dfs(i + 1, false, false, lc) % mod;
            int limit = pre ? str[i] - '0' : 9;
            for (int x = 1 - isnum; x <= limit; ++x) {
                if (abs(x - lc) == 1 || lc == -1) {
                    ret = (ret + dfs(i + 1, true, pre && x == limit, x)) % mod;
                }
            }
            if (!pre && isnum) {
                memo[i][lc] = ret;
            }
            return ret;
        };
        memset(memo, -1, sizeof memo);
        return dfs(0, false, true, -1);
    }
    int countSteppingNumbers(string low, string high) {
        int ret =  calc(high) - calc(low);
        // cout << ret << endl;
        int lc = -1, n = low.size();
        bool step = true;
        for (int i = 0; i < n; ++i) {
            if (lc == -1 || abs((low[i] - '0') - lc) == 1) {
                lc = low[i] - '0';
            } else {
                step = false;
                break;
            }
        }
        return (ret + mod + step) % mod;
    }
};
```