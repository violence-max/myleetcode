题目描述：

![image](/algorithmn/dynamic_programming/image/image67.png)

解决过程：

第350场周赛t3，没做出来

- 回溯+记忆化搜索
- 递推

代码：（回溯→递推）

```cpp
class Solution {
public:
    const int MOD = 1e9 + 7;

    int dfs(int i, int j, vector<vector<int>>& memo, const vector<int>& nums) {
        if (i == 0) {
            // 没有数字剩余，即寻找到一种可能的排列
            return 1;
        }
        if (memo[i][j] != -1) {
            return memo[i][j];
        }
        int res = 0;
        for (int k = 0; k < nums.size(); ++k) {
            if (((i >> k) & 1) && (nums[j] % nums[k] == 0 || nums[k] % nums[j] == 0)) {
                res = (res + dfs(i ^ (1 << k), k, memo, nums)) % MOD;
            }
        }
        return memo[i][j] = res;
    }

    int specialPerm(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> memo (1 << n, vector<int> (n, -1));    
        int res = 0;
        int mask = (1 << n) - 1;
        for (int i = 0; i < n; ++i) {
            res = (res + dfs(mask ^ (1 << i), i, memo, nums)) % MOD;
        }
        return res;
    }
};
```

```cpp
class Solution {
public:
    int specialPerm(vector<int>& nums) {
        int n = nums.size();
        int mask = 1 << n;
        vector<vector<int>> f (mask, vector<int> (n, 0));
        for (int i = 0; i < n; ++i) {
            f[0][i] = 1; // 模拟dfs的边界情况，如果没有数字剩余则说明达到了一个有效的排列
        }
        const int MOD = 1e9 + 7;
        for (int i = 1; i < mask; ++i) {
            for (int k = 0; k < n; ++k) {
                if ((i >> k) & 1) {
                    // 在i中尝试挑取k
                    for (int j = 0; j < n; ++j) {
                        if (!((i >> j) & 1) && nums[k] % nums[j] == 0 || nums[j] % nums[k] == 0) {
                            // 上一次选择的数字一定不在i中
                            f[i][j] = (f[i][j] + f[i ^ (1 << k)][k]) % MOD; 
                        }
                    }
                }
            }
        }
        int res = 0;
        for (int i = 0; i < n; ++i) {
            res = (res + f[(mask - 1) ^ (1 << i)][i]) % MOD;
        }
        return res;
    }
};
```