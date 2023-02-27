题目描述：  
![image](/basical/array/image/image54.png)  
解决过程：  
靠，真的是太久没做题了，从想法的出现到代码的落地花了我三十多分钟  
算法要点：  
1. 分奇偶讨论
2. 可以用辅助函数helper(nums,pos)，pos在循环里每次自增2来实现对奇数或者偶数的讨论  
代码:  
```cpp
class Solution {
public:
    int movesToMakeZigzag(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp (2,vector<int> (n,0));
        for (int i = 0; i < n; i++) {
            if (i & 1) {
                int even = min(i > 0 ? nums[i-1] : nums[i] + 1, i < n - 1 ? nums[i+1] : nums[i] + 1);
                int diff = nums[i] - even;
                dp[0][i] = ((i > 0) ? dp[0][i-1] : 0) + (diff >= 0 ? diff + 1 : 0);   
                if (i < n - 1) dp[0][i+1] = dp[0][i];                
            } else {
                int odd = min(i > 0 ? nums[i-1] : nums[i] + 1, i < n - 1 ? nums[i+1] : nums[i] + 1);
                int diff = nums[i] - odd;
                dp[1][i] = (i > 0 ? dp[1][i-1] : 0) + (diff >= 0 ? diff + 1 : 0);
                if (i < n - 1) dp[1][i+1] = dp[1][i];
            }
        }
        return min(dp[0][n-1], dp[1][n-1]);
    }
};
```