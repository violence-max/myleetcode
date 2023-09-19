题目描述：

![image](/basical/array/image/image108.png)

解决过程：

第113场双周赛t2，没做出来

- 众数、构造、二分

代码：

```cpp
class Solution {
public:
    int minLengthAfterRemovals(vector<int>& nums) {
        int n = nums.size();
        int mid = nums[n/2];
        int mxcnt = upper_bound(nums.begin(), nums.end(), mid) - lower_bound(nums.begin(), nums.end(), mid);
        return max(2 * mxcnt - n, n & 1);
    }
};
```