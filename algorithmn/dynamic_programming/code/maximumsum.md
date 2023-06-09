题目描述：

![image](/algorithmn/dynamic_programming/image/image63.png)

解决过程：

没做出来

- 动态规划：dp[i][0]表示以下标i结尾但不删除元素的子数组最大和，dp[i][1]表示以下标i结尾的删除元素的子数组最大和

代码：

```cpp
class Solution {
public:
    int maximumSum(vector<int>& arr) {
        int n = arr.size();
        int dp0 = arr[0], dp1 = 0;
        int res = dp0;
        for (int i = 1; i < n; ++i) {
            dp1 = max(dp0, dp1 + arr[i]);
            dp0 = max(dp0, 0) + arr[i];
            res = max(res, max(dp0, dp1));
        }
        return res;
    }
};
```