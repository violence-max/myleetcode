题目描述：  
![image](/algorithmn/dynamic_programming/image/image21.png)  
解题过程：  
简单，虽然一开始把这个题看成另一个题了，以为可以多次交易来着。但是后面还是在稍微花了一点时间的情况下把这个题做出来了，就是考虑如果每天买入股票然后下一天卖出能得到多少最大利润而已。  
代码：  
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int buy = - prices[0],nobuy = 0;
        for (int i = 1; i < n; i++) {
            nobuy = max(nobuy,buy+prices[i]);
            buy = max(buy,-prices[i]);
        }
        return nobuy;
    } 
};
```