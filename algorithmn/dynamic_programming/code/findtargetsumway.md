题目描述：  
![image](/algorithmn/dynamic_programming/image/image11.png)  
解题过程：  
知道这是一个背包问题，但是没有想到我对背包问题的了解还是太少，花了40分钟去推导这个玩意，我以为我又发现了什么新的东西，一看题解，原来是我对选或者不选压根就没有了解。  
代码：  
```cpp
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int n = nums.size();
        int sum = accumulate(nums.begin(),nums.end(),0);
        target = sum + target;
        if (target & 1 || target > 2 * sum || target < 0) return 0;
        target /= 2;
        vector<int> dp(target+1);
        dp[0] = 1;
        for (int n : nums) {
            for (int j = target; j >= n; j--) {
                dp[j] = dp[j] + dp[j-n];
            }
        }
        return dp[target];
    }
};
```