题目描述：  
![image](/algorithmn/dynamic_programming/image/image15.png)  
解决过程：  
哎，背包问题，自己不知道咋整，开始用贪心的方法，拿最大的开始尝试，然后求余，看余数在不在数组里面。就是一个很easy的排序+贪心的思路。但是啊，我还是想得太简单了，因为我在测试的时候出现了两个问题：只选最后一个值和总是把最后一个值选得很大。这两个问题都会有对应的测试用例证明这样选是错误的。不过也还好了，一个小时的时间练习了一下coding的能力和复习了一下二分查找（找小于等于目标值的）  
代码：  
动态规划：  
动态规划方程啊，还是太难了对于我来说。这个题又是一个背包重量对着物品遍历的经典动态规划问题，强无敌！  
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        if (amount == 0) return 0;
        vector<int> dp(amount+1,INT_MAX);
        dp[0] = 0;
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i && dp[i-coin] < INT_MAX) {
                    dp[i] = min(dp[i],dp[i-coin]+1);
                }
            }
        }
        return dp[amount] == INT_MAX ? -1 : dp[amount];
    }
};
```  
记忆化搜索：  
这个方法的递归树实在是太优雅了，我承认我湿了  
```cpp
class Solution {
private:
    vector<int> count;
public:
    int dp(vector<int> coins,int rem) {
        if (rem < 0) return -1;
        if (rem == 0) return 0;
        if (count[rem-1] != 0) return count[rem-1];
        int MIN = INT_MAX;
        for (int coin : coins) {
            int res = dp(coins,rem-coin);
            if (res >= 0 && res < MIN) {
                MIN = res + 1;
            }
        }
        count[rem-1] = MIN == INT_MAX ? -1 : MIN;
        return count[rem-1];
    }

    int coinChange(vector<int>& coins, int amount) {
        if (amount == 0) return 0;
        count.resize(amount);
        return dp(coins,amount);
    }
};
```  
自己的不能通过测试的版本  
```cpp
class Solution {
public:
    int binarysearch(const vector<int>& coins,int coin,int edge) {
        int left = 0,right = edge - 1;
        int index = -1;
        while (left <= right) {
            int mid = (left + right) >> 1;
            if (coins[mid] > coin) {
                right = mid - 1;
            } else {
                index = mid;
                left = mid + 1;
            }
        }
        return index;
    }

    int dfs(const vector<int> &coins,int amount,int n) {
            int index = binarysearch(coins,amount,n);
            if (index == -1) return -1;
            int ret = amount / coins[index];
            int mod = amount % coins[index];
            if (mod == 0) {
                return ret;
            }
            else {
                while (mod) {
                    index = binarysearch(coins,mod,index);
                    if (index == -1) return -1;
                    else if (coins[index] == mod) {
                        return ret + 1;
                    }
                    else {
                        ret += mod / coins[index];
                        mod %= coins[index];
                        if (mod == 0) {
                            return ret;
                        }
                    }
                }
                return -1;
            }
    }

    int coinChange(vector<int>& coins, int amount) {
        if (amount == 0) return 0;
        sort(coins.begin(),coins.end());
        int n = coins.size();
        for (int i = n; i >= 0; i--) {
            int ret = dfs(coins,amount,i);
            if (ret == -1) continue;
            else return ret;
        }
        return -1;
    }
};
```