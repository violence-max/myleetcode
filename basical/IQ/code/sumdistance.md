题目描述：

![image](/basical/IQ/image/image43.png)

解决过程：

第107场双周赛t3，没做出来

代码：

```cpp
class Solution {
public:
    int sumDistance(vector<int>& nums, string s, int d) {
        const int MOD = 1e9 + 7;
        int n = nums.size();
        vector<long long> a (n);
        for (int i = 0; i < n; ++i) {
            // 直接无视机器人的碰撞
            // 注意2e9+1e9溢出了
            a[i] = (long long)nums[i] + (s[i] == 'R' ? d : -d);
        }
        sort(a.begin(), a.end());
        long long res = 0, sum = 0;
        for (int i = 0; i < n; ++i) {
            // res += (long long)(i + 1) * (n - i - 1) * (a[i + 1] - a[i]) % MOD, sum = 0; 
            // 这种两边端点的方式会溢出

            // 递推计算两点之间的距离：
            // a[i]之前共有i个机器人，距离之和为：(a[i] - a[0]) + (a[i] - a[1]) + ... + (a[i] - a[i-1]) \
            // = i * a[i] - (a[0] + a[1] + ... + a[i-1])
            res = (res + i * a[i] - sum) % MOD;
            sum += a[i];
        }
        return res;
    }
};
```