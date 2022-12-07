题目描述：  
![image](/algorithmn/dynamic_programming/image/image23.png)  
解决过程：  
靠，这道题真的难到我了，我以为要维护非常多的状态然后就没有敢看下去就直接看题解了，没有想到这是动态规划啊！可以用数组和方程来解决这些问题！所以用一个数组来维护买入的状态，用一个数组来维护卖出的状态就可以了。有些细节是真的强无敌，比如最多只能发生几次交易，在进行循环的时候对于每一天，循环到底是进行几次交易，而且在对交易次数循环之前，还要先对交易次数为0的情况先进行赋值，强无敌！  
代码：  
```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        if ( prices.empty()) return 0;
        int n = prices.size();
        int k = min(k,n / 2);
        vector<int> buy (k+1);
        vector<int> sell(k+1);
        buy[0] = - prices[0];
        sell[0] = 0;
        for (int i = 1; i <= k; i++) {
            buy[i] =  sell[i] = INT_MIN / 2;
        }
        for (int i = 1; i < n; i++) {
            buy[0] = max(buy[0],sell[0]-prices[i]);
            for (int j = 1; j <= k; j++) {
                buy[j] = max(buy[j],sell[j]-prices[i]);
                sell[j] = max(sell[j],buy[j-1]+prices[i]);
            }
        }
        return *max_element(sell.begin(),sell.end());
    }
};
```