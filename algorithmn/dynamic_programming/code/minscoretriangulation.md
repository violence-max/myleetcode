题目描述：  
![image](/algorithmn/dynamic_programming/image/image54.png)  
解决过程：  
没做出来  
- 区间dp
1. dp[i][j]的定义为values数组下标从i到j（左闭右闭）按照三角划分的规则可以得到的最低分数
2. 最后返回dp[0][n-1]即可
3. k从i+1遍历至j-1，每次遍历都可以划分为三个区间：i、j、k构成的三角形，区间[i,j]，区间[k,j]  
代码：  
```cpp
class Solution {
public:
    int minScoreTriangulation(vector<int>& values) {
        unordered_map<int,int> memo;
        // dp[i][j]的定义为values数组下标从i到j（左闭右闭）按照三角划分的规则可以得到的最低分数
        function<int(int,int)> dp = [&](int i, int j) -> int {
            // j >= i + 2
            if (i + 2 > j) {
                return 0;
            }
            if (i + 2 == j) {
                return values[i] * values[i+1] * values[i+2];
            }
            int key = i * values.size() + j; // 建立哈希键值
            if (memo.count(key)) {
                return memo[key];
            } else {
                int minScore = INT_MAX;
                for (int k = i + 1; k < j; k++) {
                    minScore = min(minScore, values[i] * values[k] * values[j] + dp(i,k) + dp(k,j));
                }
                return memo[key] = minScore;
            }
        };
        return dp(0,values.size() - 1);
    }
};
```