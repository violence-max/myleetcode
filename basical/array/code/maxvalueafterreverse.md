题目描述：

![image](/basical/array/image/image79.png)

解决过程：

考虑了分类讨论，但是对于最难的情况没有想到去掉绝对值去分类讨论

代码：

```cpp
class Solution {
public:
    int maxValueAfterReverse(vector<int>& nums) {
        int n = nums.size();
        if (n == 1) {
            return 0;
        }

        int sum = 0;
        for (int i = 0; i < n - 1; ++i) {
            sum += abs(nums[i + 1] - nums[i]);
        }

        int mx1 = INT_MIN;
        for (int i = 1; i < n - 1; ++i) {
            // 分别以nums[0]为左端点和以nums[n-1]为右端点进行翻转

            mx1 = max(mx1, abs(nums[i + 1] - nums[0]) - abs(nums[i + 1] - nums[i]));
            mx1 = max(mx1, abs(nums[i - 1] - nums[n - 1]) - abs(nums[i - 1] - nums[i]));
        }

        int mx2 = INT_MIN, mn2 = INT_MAX;
        for (int i = 0; i < n - 1; ++i) {
            // 遍历所有数对，记录所有数对中最小值的最大值mx2和最大值的最小值mn2
            // 在两个端点处于(0, n - 1)范围内的子数组的翻转，将两个数对的四个数从小到大排序为a,b,c,d,
            // 再讨论去掉绝对值以后的6种情况，可以得到交换后能使总值变大的情况只有可以增加2 * (c - b)
            // 的情况，即两个数对中其中一个的最小值大于另外一个的最大值的情况

            mx2 = max(mx2, min(nums[i], nums[i + 1]));
            mn2 = min(mn2, max(nums[i], nums[i + 1]));
        }

        return max(sum, sum + max(mx1, 2 * (mx2 - mn2)));
    }
};
```