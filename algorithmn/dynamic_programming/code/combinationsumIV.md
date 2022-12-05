题目描述：  
![image](/algorithmn/dynamic_programming/image/image14.png)  
解题过程：  
我靠，我以为这个题跟兑换零钱II一模一样，结果不行，仔细一看，居然要考虑顺序？我整个人就无语了，无论在草稿纸上怎么递推，根本就不能得到一个很好的考虑顺序的方法。  
看了题解，真的，我第一次做背包问题把背包重量放在外层递归，但是这个样子的效果实在是太好了。其实考虑顺序也是相当简单，只需要考虑把选上当前物品的时候，这个被选的物品放在之前已经选好了的物品的序列的最后就可以了，所以在考虑每个重量的时候，对于所有物品，都要考虑当前物品是否可以放在最后，把这些值全部加起来，无敌。  
没有想到这个题还有整数范围溢出的问题，我直接无语。尝试过程大致是：把类型改成long→把赋值右值强转成long→看题解，发现赋值右值有可能超过整数最大值，因此要多一个判断，加了一个相加小于整数最大值的判断→打脸，如果两个数相加都大于整数最大值了，那这个式子就废了，所以必须是一个值小于整数最大值减去另外一个值的差  
代码：  
```cpp
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target+1);
        dp[0] = 1;
        for (int i = 1; i <= target; i++) {
            for (int num : nums) {
                if (num <= i && dp[i-num] < INT_MAX - dp[i]) {
                    dp[i] += dp[i - num];
                }
            }
        }
        return dp[target];
    }
};
```