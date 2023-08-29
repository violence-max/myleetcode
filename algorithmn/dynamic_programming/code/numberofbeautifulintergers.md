题目描述：

![image](/algorithmn/dynamic_programming/image/image88.png)

解题过程：

第111场双周赛t4，没做出来

数位dp

代码：

```cpp
class Solution {
public:
    int memo[15][30][30];
    int calc(int high, int k) {
        string str = to_string(high);
        int len = str.size();
        memset(memo, -1, sizeof memo);
        function<int(int,int,int,bool,int)> dfs;
        dfs = [&](int idx, int val, int diff, bool pre, bool is_num) -> int {
            if (idx == len) {
                return is_num && val == 0 && diff == len;
            } else if (!pre && is_num && memo[idx][val][diff] != -1) return memo[idx][val][diff];

            int ret = 0;
            if (!is_num)
                ret = dfs(idx + 1, val, diff, false, false);
            int limit = pre ? str[idx] - '0' : 9;
            for (int x = 1 - is_num; x <= limit; ++x) {
                ret += dfs(idx + 1, (val * 10 + x) % k, diff + x % 2 * 2 - 1, 
                            pre && x == limit, true);
            }
            if (!pre && is_num) {
                memo[idx][val][diff] = ret;
            }
            return ret;
        };
        return dfs(0, 0, len, true, false);
    }
    int numberOfBeautifulIntegers(int low, int high, int k) {
        return calc(high, k) - calc(low - 1, k);
    }
};
```