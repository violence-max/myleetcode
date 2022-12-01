题目描述：  
![image](/algorithmn/greed/image/image3.png)  
解题过程：  
自己想用贪心的方法一次扫描做出来，发现贪心没有办法模拟股票的买卖。  
看了题解，对于贪心的解法，确实，不能模拟股票的买卖，只能计算价格数组里面所有升序的子序列之差，最后把这些差值加起来，得到的值一定是最大利润。但是不能知道要什么时候买入，什么时候卖出。 
这道题还可以用动态规划去做，为每一天去维护若当天持有股票的最大利润和若当天没有股票的最大利润，很秀。  
代码：  
贪心：  
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int length = prices.size();
        int totalprofit = 0;
        for (int i = 1; i < length; i++) {
            totalprofit += max(0,prices[i] - prices[i-1]);
        }
        return totalprofit;
    }
};
```  
动态规划：  
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int length = prices.size();
        int buyProfit = - prices[0],normalProfit = 0;
        for (int i = 1; i < length; i++) {
            int newBuyProfit = max(normalProfit-prices[i],buyProfit);
            int newNormalProfit = max(buyProfit+prices[i],normalProfit);
            normalProfit = newNormalProfit;
            buyProfit = newBuyProfit;
        }
        return normalProfit;
    }
};
```