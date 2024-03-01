题目描述：

![image](/algorithmn/dynamic_programming/image/image104.png)

解决过程：

第369场周赛t3，没做出来

动态规划经典选或不选的思路

代码：

```cpp
class Solution {
public:
    vector<array<long long , 3>> memo;
    long long dfs(vector<int>& nums, int i, int j, int k) {
        if (i < 0) return 0;
        long long &res = memo[i][j];
        if (res != -1) return res;
        res = dfs(nums, i - 1, 0, k) + max(k - nums[i] , 0);
        if (j < 2) res = min(res, dfs(nums, i - 1, j + 1, k));
        return res;
    }
    long long minIncrementOperations(vector<int>& nums, int k) {
        int n = nums.size();
        memo = vector<array<long long, 3>> (n, {-1, -1, -1});
        return dfs(nums, n - 1, 0, k);
    }
};
```