题目描述：

![image](/basical/array/image/image115.png)

解决过程：

第366场周赛t4，没做出来

- 位运算性质构造题

代码：

```cpp
class Solution {
public:
    static const int l = 30;
    int cnt[l];
    int maxSum(vector<int>& nums, int k) {
        // 拆位
        int n = nums.size();
        memset(cnt, 0, sizeof cnt);
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < l; ++j) {
                cnt[j] += (nums[i] & (1 << j)) ? 1 : 0;
            }
        }
        // for (int i = 0; i < l; ++i) cout << i << ' ' << cnt[i] << endl;
        long long ans = 0;
        int mod = 1e9 + 7;
        while (k--) {
            long long x = 0;
            for (int i = 0; i < l; ++i) {
                if (cnt[i]) {
                    x |= 1 << i;
                    cnt[i]--;
                }
            }
            // cout << x << endl;
            ans = (ans + x % mod * x) % mod;
        }
        return ans;
    }
};
```