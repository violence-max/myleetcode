题目描述：

![image](/algorithmn/dynamic_programming/image/image98.png)

解决过程：

第115场双周赛t4，没做出来

- 多重背包问题

代码：

```cpp
class Solution {
public:
    int countSubMultisets(vector<int> &nums, int l, int r) {
        const int MOD = 1e9 + 7;
        int total = 0;
        unordered_map<int, int> cnt;
        for (int x: nums) {
            total += x;
            cnt[x]++;
        }
        if (l > total) {
            return 0;
        }

        r = min(r, total);
        vector<int> f(r + 1);
        f[0] = cnt[0] + 1;
        cnt.erase(0); // 是否删除对于答案没有影响，但是是不必要的循环

        int sum = 0;
        for (auto [x, c]: cnt) {
            auto new_f = f;
            sum = min(sum + x * c, r); // 到目前为止，能选的元素和至多为 sum
            // cout << sum << endl;
            for (int j = x; j <= sum; j++) { // 把循环上界从 r 改成 sum 可以快不少
                // f[i][j] = f[i][j-1] + f[i-1][j] - f[i-1][j - (c+1) * x], 由于对上一行的旧值有依赖也对新求出来的值有依赖，这里用了new_f
                new_f[j] = (f[j] + new_f[j - x]) % MOD;
                // f[j] = (f[j] + f[j - x]) % MOD;
                if (j >= (c + 1) * x) {
                    new_f[j] = (new_f[j] - f[j - (c + 1) * x] + MOD) % MOD; // 避免减法产生负数
                    // f[j] = (f[j] - f[j - (c + 1) * x] + MOD) % MOD; // 避免减法产生负数
                }
            }
            f = move(new_f);
        }

        int ans = 0;
        for (int i = l; i <= r; i++) {
            // cout << i << ' ' << f[i] << endl;
            ans = (ans + f[i]) % MOD;
        }
        return ans;
    }
};
```