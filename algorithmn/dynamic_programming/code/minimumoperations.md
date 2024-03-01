题目描述：

![image](/algorithmn/dynamic_programming/image/image89.png)

解决过程：

第111场双周赛t3，ac

可以转换成最多保留多少个元素不变，则这些元素都是非递减的

- 枚举答案的动态规划
- LIS裸题

代码：（枚举答案→LIS）

```cpp
class Solution {
public:
    int dp[100020];
    int minimumOperations(vector<int>& nums) {
        int n = nums.size(), len = 0;
        for (int i = 0; i < n; ++i) {
            int mxx = 0;
            for (int j = 0; j < i; ++j) {
                if (dp[j] > mxx && nums[j] <= nums[i]) {
                    mxx = dp[j];
                }
            }
            dp[i] = mxx + 1;
            len = max(len, dp[i]);
        }
        return n - len;
    }
};
```

```cpp
class Solution {
public:
    int dp[200][200];
    int minimumOperations(vector<int>& nums) {
        int n = nums.size();
        memset(dp, -1, sizeof dp);
        dp[0][0] = 0;
        for (int i = 1; i <= n; ++i) {
            for (int k = 0; k <= i; ++k) {
                if (dp[i-1][k] != -1 && nums[i-1] >= dp[i-1][k]) {
                    dp[i][k] = nums[i-1];
                }
                if (k > 0 && dp[i-1][k-1] != -1) {
                    dp[i][k] = dp[i][k] != -1 ? min(dp[i-1][k-1], dp[i][k]) : dp[i-1][k-1];
                }
                
                if (i == n && dp[i][k] != -1) return k;
            }
        }
        return -1;
    }
};
```