题目描述：  
![image](/algorithmn/dynamic_programming/image/image13.png)  
解题过程：  
今天终于开张了，来到了完全背包问题我发现我对背包问题的抽象又进一步理解了，一遍过，舒服得雅痞  
代码：  
```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        int n = coins.size();
        vector<int> dp(amount+1);
        dp[0] = 1;
        for (int n : coins) {
            for (int i = n; i <= amount; i++) {
                dp[i] = dp[i]+dp[i-n];
            }
        }
        return dp[amount];
    }
};
```