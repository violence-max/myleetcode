题目描述：

![image](/algorithmn/greed/image/image25.png)

解决过程：

第355场周赛t2，ac

- 贪心

代码：

```cpp
class Solution {
public:
    long long maxArrayValue(vector<int>& nums) {
        int n = nums.size();
        long long sum = nums[n-1];
        for (int i = n - 2; i >= 0; --i) {
            if (nums[i] <= sum) {
                sum += (long long)nums[i];
            } else {
                sum = nums[i];
            }
        }
        return sum;
    }
};
```