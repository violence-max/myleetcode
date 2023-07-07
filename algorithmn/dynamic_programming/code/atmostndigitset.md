题目描述：

![image](/algorithmn/dynamic_programming/image/image73.png)

解决过程：

数位dp

代码：

```cpp
bool can[10];
string model;
int memo[11][10];
int len;
class Solution {
public:
    int atMostNGivenDigitSet(vector<string>& digits, int n) {
        memset(can, 0, sizeof can);
        memset(memo, -1, sizeof memo);
        for (const auto& str : digits) {
            int id = str[0] - '0';
            can[id] = true;
        }
        can[0] = true;
        model = to_string(n);
        len = model.size();
        function<int(const string&, int, bool, int, int)> dfs = [&](const string& s, int id, bool flag, int mask, int l) -> int {
            if (!flag && memo[id][l] != -1) {
                return memo[id][l];
            } else if (id == len) {
                return mask == 0 ? 0 : 1;
            }

            int res = 0;
            int limit = flag ? s[id] - '0' : 9;
            for (int i = 0; i <= limit; ++i) {
                if ((mask == 0 && i == 0) || (i != 0 && can[i])) {
                    // cout << id << ' ' << i << endl;
                    res += dfs(s, id + 1, flag && i == limit, mask == 0 && i == 0 ? 0 : 1, i);
                }
            }
            if (!flag) {
                memo[id][l] = res;
            }
            return res;
        };
        return dfs(model, 0, true, 0, 0);
    }
};
```