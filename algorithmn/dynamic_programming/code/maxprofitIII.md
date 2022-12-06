题目描述：  
![image](/algorithmn/dynamic_programming/image/image22.png)  
解题过程：  
想出了一种蠢到离谱的办法：在每个不减区间里面拿最大值减去最小值作为利润，最多只有两个利润，没新增一个利润都挑出最大的两个利润，调了四十多分钟，虽然通过了接近两百个测试，但是还是有测试选择了最大的两个利润出现在两个相邻的不减区间里面—有三个不减区间，利润最大值是第二个不减区间的最大值减去第一个不见区间的最小值的差加上第三个不减区间的利润。简直离谱！！根本没有想到这个。  
代码：  
动态规划版本：  
四个状态简直烧得不行，包括初始时状态的设定和赋值以及在转换过程中对于状态的更新，都可以好好品味品味一下  
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int buy1 = - prices[0],sell1 = 0;
        int buy2 = - prices[0],sell2 = 0;
        for (int i = 1; i < n; i++) {
            buy1 = max(buy1,-prices[i]);
            sell1 = max(sell1,buy1+prices[i]);
            buy2 = max(buy2,sell1-prices[i]);
            sell2 = max(sell2,buy2+prices[i]);
        }
        return sell2;
    }
};
```  
自己的破产版：  
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        int buy = prices[0],index = 0,noBuy;
        int count = 0;
        int profit1 = 0,profit2 = 0;
        while (index < n) {
            int p = index + 1;
            while (p < n) {
                if  (prices[p-1] > prices[p] || p == n-1) {
                    noBuy = (p == n-1 && prices[p-1] <= prices[p]) ? prices[p] - buy : prices[p-1]-buy;
                    count++;
                    if (count == 1) profit1 = noBuy;
                    else if (count == 2) {
                        if (noBuy > profit1) profit2 = noBuy;
                        else {
                            profit2 = profit1;
                            profit1 = noBuy;
                        }
                    }
                    else {
                        if (noBuy > profit2) {
                            profit1 = profit2;
                            profit2 = noBuy;
                        } else {
                            profit1 = (noBuy > profit1) ? noBuy : profit1;
                        }
                    }
                    break;
                }
                p++;
            }
            if (p == n-1) break;
            index = p;
            buy = prices[p];
        }
        return profit1 + profit2;
    }
};
```