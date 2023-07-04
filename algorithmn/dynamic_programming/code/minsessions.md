题目描述：

![image](/algorithmn/dynamic_programming/image/image72.png)

解决过程：

写了一个小时，吐了，明明知道怎么写，就是没写对

代码：（回溯→递推）

```cpp
int memo[1 << 14][15];
class Solution {
public:
    int s;
    int minSessions(vector<int>& tasks, int sessionTime) {
        s = sessionTime;
        int n = tasks.size();
        memset(memo, -1, sizeof memo);
        function<int(int, int)> dfs = [&](int mask, int sum) -> int {
            if (mask == 0) {
                return sum != 0;
            } else if (memo[mask][sum] != -1) {
                return memo[mask][sum];
            }
            
            // cout << mask << ' ' <<  sum << endl;
            int res = 0x3f3f3f3f;
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    int judge = tasks[i] + sum - s;
                    int nextsum = judge < 0 ? tasks[i] + sum : judge == 0 ? 0 : tasks[i];
                    int tempres = dfs(mask ^ (1 << i), nextsum) + (judge >= 0);
                    res = min(res, tempres);
                }
            }
            return memo[mask][sum] = res == 0x3f3f3f3f ? -1 : res;
        };
        return dfs((1 << n) - 1, 0);
    }
};
```

```
class Solution {
public:
    int minSessions(vector<int>& tasks, int sessionTime) {
        int n = tasks.size();
        vector<int> valid(1 << n);
        for (int mask = 1; mask < (1 << n); ++mask) {
            int needTime = 0;
            for (int i = 0; i < n; ++i) {
                if (mask & (1 << i)) {
                    needTime += tasks[i];
                }
            }
            if (needTime <= sessionTime) {
                valid[mask] = true;
            }
        }

        vector<int> f(1 << n, INT_MAX / 2);
        f[0] = 0;
        for (int mask = 1; mask < (1 << n); ++mask) {
            for (int subset = mask; subset; subset = (subset - 1) & mask) {
                if (valid[subset]) {
                    f[mask] = min(f[mask], f[mask ^ subset] + 1);
                }
            }
        }
        return f[(1 << n) - 1];
    }
};
```