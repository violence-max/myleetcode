题目描述：

![image](/algorithmn/dynamic_programming/image/image75.png)

解决过程：

没做出来

- 序列式dp

代码：

```cpp
class Solution {
public:
    long long maxAlternatingSum(vector<int>& nums) {
        long long t = nums[0], s = 0;
        for (int i = 1; i < nums.size(); ++i) {
            t = max(t, s + nums[i]);
            s = max(s, t - nums[i]);
        }
        return t;
    }
};
```