题目描述：  
![image](/algorithmn/dynamic_programming/image/image9.png)  
解题过程：  
牛逼啊，动态规划分区看来难题真的可以很多，又是一道完全不会的题目。  
看了题解，居然是一道01背包问题，而且居然有那么多可以提前判断的条件，最重要的是—以所有值的和的一半当作目标的思想太骚了。  
直接上代码，分别是没有做空间优化的和做了空间优化的：  
```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return false;
        int sum  = accumulate(nums.begin(),nums.end(),0);
        if (sum & 1) return false;
        int maxNum = *max_element(nums.begin(),nums.end());
        int target = sum / 2;
        if (maxNum > target) return false;
        vector<vector<bool>> dp(n,vector<bool> (target+1));
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }
        dp[0][nums[0]] = true;
        for (int i = 1; i < n; i++) {
            int num = nums[i];
            for (int j = 1; j <= target; j++) {
                if (j >= nums[i]) {
                    dp[i][j] = dp[i-1][j] | dp[i-1][j-num];
                } else dp[i][j] = dp[i-1][j];
            }
        }
        return dp[n-1][target];
    }
};
```

```cpp
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) return false;
        int sum  = accumulate(nums.begin(),nums.end(),0);
        if (sum & 1) return false;
        int maxNum = *max_element(nums.begin(),nums.end());
        int target = sum / 2;
        if (maxNum > target) return false;
        vector<bool> dp(target+1);
        dp[0] = true;
        for ( int i = 1; i < n; i++) {
            int num = nums[i];
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] | dp[j - num];
            }
        }
        return dp[target];
    }
};
```