题目描述：

![image](/algorithmn/dynamic_programming/image/image106.png)

解决过程：

没做出来

代码：

```cpp
class Solution {
public:
    bool validPartition(vector<int>& nums) {
        int n = nums.size();
        vector<bool> dp (n + 1, false);
        dp[0] = true;
        for (int i = 1; i <= n; ++i) {
            if (i >= 2) {
                dp[i] = dp[i-2] && (nums[i-1] == nums[i-2]);
            } 
            if (i >= 3 && !dp[i]) {
                dp[i] = dp[i-3] && ((nums[i-3] == nums[i-2] && nums[i-2] == nums[i-1]) || (nums[i-3] + 1 == nums[i-2] && nums[i-2] + 1 == nums[i-1]));
            }
        }
        return dp[n];
    }
};
```