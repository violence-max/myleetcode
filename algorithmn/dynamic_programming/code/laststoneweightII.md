题目描述：  
![image](/algorithmn/dynamic_programming/image/image10.png)  
解题过程：  
我靠，又是完全不会，我太喜欢动态规划了。  
没想到这也是01背包问题，把石头分成两拨，根据公式让被减掉的那一拨石头尽可能的大，但是不能超过总重量一半的向下取整部分，强无敌啊。  
自己写滚动数组的时候居然忽略了初始赋值一个true  
代码：  
```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        int n = stones.size();
        int sum = accumulate(stones.begin(),stones.end(),0);
        int mid = sum / 2;
        vector<bool> dp(mid+1);
        dp[0] = true;
        for (auto weight : stones) {
            for (int j = mid; j >= weight; j--) {
                dp[j] = dp[j] | dp[j-weight];
            }
        }
        for (int j = mid; ; j--) {
            if (dp[j]) return sum - 2 * j;
        }
    }
};
```