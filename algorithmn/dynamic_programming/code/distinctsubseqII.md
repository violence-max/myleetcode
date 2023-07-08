题目描述：

![image](/algorithmn/dynamic_programming/image/image74.png)

解题过程：

想到的是f[i]代表0…i序列的子序列个数，没想到以s[i]结尾的子序列个数

- 动态规划

代码：

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;
    int n;
    int id[26];
    int f[2020];
    int distinctSubseqII(string s) {
        n = s.size();
        memset(id, -1, sizeof id);
        for (int i = 0; i < n; ++i) {
            f[i] = 1;
            for (int j = 0; j < 26; ++j) {
                if (id[j] != -1) {
                    f[i] = (f[i] + f[id[j]]) % MOD;
                }
            }
            id[s[i] - 'a'] = i;
        }
        int ans = 0;
        for (int i = 0; i < 26; ++i) {
            if (id[i] != -1) {
                ans = (ans + f[id[i]]) % MOD;
            }
        }
        return ans;
    }
};
```