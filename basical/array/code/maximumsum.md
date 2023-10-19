题目描述：

![image](/basical/array/image/image112.png)

解决过程：

第363场周赛t4，没做出来

- 质因数分解

代码：

```cpp
class Solution {
public:
    int core(int n) { // 求n分解出来的质因数组合
        int res = 1;
        for (int i = 2; i * i <= n; ++i) { // 只需要枚举到根号n，因为对于两个因子而言只需要枚举较小的那个就可以得出较大的另一个
            int e = 0;
            while (n % i == 0) { // 无需考虑i是否为质数，类似于埃式筛，前面的质数会把后面的非质数过滤掉
                e ^= 1;
                n /= i;
            }
            if (e) { // 如果某个质数的出现次数是奇数
                res *= i;
            }
        }
        if (n > 1) { // 还剩余一个比较大的质数
            res *= n;
        }
        return res;
    }

    long long maximumSum(vector<int>& nums) {
        int n = nums.size();
        long long sum[n+1];
        memset(sum, 0, sizeof sum);
        for (int i = 0; i < n; ++i) {
            int temp = core(i+1);
            // cout << i << ' ' << temp << endl;
            sum[core(i+1)] += nums[i];
        }
        return *max_element(sum + 1, sum + n + 1);
    }
};
```