题目描述：  
![image](/algorithmn/dynamic_programming/image/image24.png)  
解题过程：  
哎，不知道每一天到底应该有多少个状态。动态规划的题目，虽然说最重要的应该是动态规划方程，但是在写出动态规划方程之前必须要清除每个动作会有什么效果，会出现什么状态啊  
代码：  
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        if (n == 1) return 0;
        else {
            int buy = - prices[0];
            int sell = 0;
            int freeze = 0;
            for (int i = 1; i < n; i++) {
                int temp = buy;
                buy = max(buy,sell-prices[i]);
                sell = max(sell,freeze);
                freeze = temp + prices[i];
            }
            return max(sell,freeze);
        }
    }
};
```