题目描述：  
![image](/algorithmn/dynamic_programming/image/image16.png)  
解题过程：  
这道题我是会的啊，我都知道了怎么样去弄动态规划方程了，跟零钱兑换问题一致的这个题目。但是啊，我不知道取开方数的向下取整的api，看了题解，没有想到，题解用了一种很骚的方式，甚至没有用api，j*j ≤ i，我操了，这就是leetcode  
代码：  
```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1,INT_MAX);
        dp[0] = 0;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j * j <= i; j++) {
                if (dp[i-j*j] < INT_MAX) {
                    dp[i] = min(dp[i],dp[i-j*j]+1);
                }
            }
        }
        return dp[n];
    }
};
```