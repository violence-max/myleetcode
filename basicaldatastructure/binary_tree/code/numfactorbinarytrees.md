题目描述：

![image](/basicaldatastructure/binary_tree/image/image65.png)

解决过程：

没做出来

dp[i]表示以下标i的节点为根节点所能构成的二叉树数目

代码：

```cpp
class Solution {
public:
    long long dp[1020];
    const int mod = 1e9 + 7;
    int numFactoredBinaryTrees(vector<int>& arr) {
        sort(arr.begin(), arr.end());
        int ret = 0, n = arr.size();
        for (int i = 0; i < n; ++i) {
            dp[i] = 1;
            for (int left = 0, right = i - 1; left <= right; ++left) {
                while (left <= right && (long long)arr[left] * arr[right] > arr[i]) {
                    --right;
                }
                if (left <= right && (long long)arr[left] * arr[right] == arr[i]) {
                    if (left == right) {
                        dp[i] = (dp[i] + (dp[left] % mod) * (dp[right] % mod) % mod) % mod;
                    } else {
                        dp[i] = (dp[i] + 2 * (dp[left] % mod) * (dp[right] % mod) % mod) % mod;
                    }
                }
            }
            ret = (ret + dp[i]) % mod;
        }
        return ret;
    }
};
```