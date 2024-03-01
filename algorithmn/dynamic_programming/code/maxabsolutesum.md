题目描述：

![image](/algorithmn/dynamic_programming/image/image90.png)

解决过程：

没做出来

- 考虑以nums[i]为结尾的子数组的和的最大值和最小值，绝对值必取二者其一

代码：

```cpp
class Solution {
public:
    int maxAbsoluteSum(vector<int>& nums) {
        int ans = 0;
        int p = 0, q = 0;
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            p = max(p + nums[i], nums[i]);
            q = min(q + nums[i], nums[i]);
            ans = max(ans, max(p, -q));
        }
        return ans;
    }
};
```