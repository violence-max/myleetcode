题目描述： 
![image](/algorithmn/dynamic_programming/image/image8.png) 
解题过程：  
靠，终于碰到一道一点都不会的题目了。一开始想到了遍历从1到n每个值分别以每个值为根节点的方法，但是压根不知道怎么由一个节点是根节点来求解不同二叉搜索树的数量，满脑子都是枚举。这个动态规划太寂寞了，无敌！  
代码：  
```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n+1);
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                dp[i] += dp[j-1] * dp[i-j];
            }
        }
        return dp[n];
    }
};
```