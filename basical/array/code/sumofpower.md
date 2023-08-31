题目描述：  

![英雄的力量](/basical/array/image/image101.png)

解决过程：

没做出来

排序+动态规划

代码：

```cpp
class Solution {
public:
    const int mod = 1e9 + 7;
    int sumOfPower(vector<int>& nums) {
        int res = 0;
        int dp = 0, psum = 0;
        int n = nums.size();
        sort(nums.begin(), nums.end());
        for (int i = 0; i < n; ++i) {
            dp = (nums[i] + psum) % mod;
            psum = (psum + dp) % mod;
            res = (res + (long long)nums[i] * nums[i] % mod * dp % mod) % mod;
        }
        return res;
    }
};
```