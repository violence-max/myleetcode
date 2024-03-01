题目描述：

![image](/algorithmn/dynamic_programming/image/image64.png)

解决过程：

第349场周赛t3，没做出来，没想到解决方案，尝试了各种可能，发现错在对题意的理解，其实对于巧克力的收集是并发进行的，只要获取时刻的最小值就好

代码：

```
class Solution {
public:
    long long minCost(vector<int>& nums, int x) {
        int n = nums.size();
        long long sum[n]; // sum[i] 表示操作i次总共的最小成本
        for (int i = 0; i < n; ++i) {
            // 对操作次数作循环，至多操作n-1次

            sum[i] = (long long)i * x;
        }

        for (int i = 0; i < n; ++i) {
            // 对巧克力种类进行循环

            int mn = nums[i]; // 一开始的最小成本默认为cost
            for (int j = i; j < n + i; ++j) {
                // 对操作次数进行循环，j-i代表操作次数

                // 这里其实是很优雅的递推操作
                // 假设操作x次，则第i个品种的巧克力可以获得的最小成本应该是：
                // MIN{nums[i], x + nums[(i + 1) % n], 2 * x + nums[(i + 2) % n] ...}
                // 这个循环通过从小到大循环操作次数，计算出了当操作第j - i次时，品种i可以
                // 获得的最小成本
                mn = min(mn, nums[j % n]);
                sum[j - i] += mn;
            }
        }

        return *min_element(sum, sum + n);
    }
};
```