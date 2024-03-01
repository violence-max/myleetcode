题目描述：

![image](/algorithmn/dynamic_programming/image/image83.png)

解决过程：

第353场周赛t3，ac

代码：

```cpp
class Solution {
public:
    int dp[100020][2];
    int maxNonDecreasingLength(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        int res = 1;
        memset(dp, 0, sizeof dp);
        dp[0][0] = 1;
        dp[0][1] = 1;
        for (int i = 1; i < n; ++i) {
            int o = min(nums1[i], nums2[i]), p = max(nums1[i], nums2[i]);
            int x = min(nums1[i-1], nums2[i-1]), y = max(nums1[i-1], nums2[i-1]);
            int o1 = o >= x ? dp[i-1][0] + 1 : 1;
            int p1 = o >= y ? dp[i-1][1] + 1 : 1;
            int x1 = p >= x ? dp[i-1][0] + 1 : 1;
            int y1 = p >= y ? dp[i-1][1] + 1 : 1;
            // cout << i << ' ' << o1 << ' ' << p1 << ' ' << x1 << ' ' << y1 << endl;
            dp[i][0] = max(o1, p1);
            dp[i][1] = max(x1, y1);
            res = max(res, max(dp[i][0], dp[i][1]));
        }
        return res;
    }
};
```