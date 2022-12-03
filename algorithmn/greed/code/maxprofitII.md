题目描述：  
![image](/algorithmn/greed/image/image16.png)  
解题过程：  
跟不含手续费是一样的，但是我一开始在定义买入的变量的时候居然没有加负号，还害我gdb好久。  
代码：  
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices, int fee) {
        int nobuy = 0,buy = - prices[0];
        int n = prices.size();
        for (int i = 1; i < n; i++) {
            int newNoBuy = max(nobuy,buy+prices[i]-fee);
            int newBuy = max(buy,nobuy-prices[i]);
            nobuy = newNoBuy;
            buy = newBuy;
        }
        return nobuy;
    }
};
```