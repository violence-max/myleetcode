题目描述：

![image](/algorithmn/dynamic_programming/image/image82.png)

解决过程：

第353场周赛t2，ac

代码：

```cpp
class Solution {
public:
    int memo[1020];
    int maximumJumps(vector<int>& nums, int target) {
        int n = nums.size();
        int cnt = 0;
        memset(memo, -1, sizeof memo);
        function<int(int)> dfs = [&](int i) -> int {
            if (memo[i] != -1) return memo[i];
            int res = 0;
            for (int j = i + 1; j < n; ++j) {
                if (abs(nums[j] - nums[i]) <= target) {
                    if (j == n - 1) return max(1, res);
                    int temp = dfs(j);
                    memo[j] = temp;
                    if (temp != 0) {
                        // cout << i << ' ' << j << ' ' << res << ' ' << temp << endl;
                        res = max(res, temp + 1);
                    }
                }
            }
            return memo[i] = res;
        };
        cnt = dfs(0);
        return cnt == 0 ? -1 : cnt;
    }
};
```