题目描述：

![image](/basical/array/image/image119.png)

解决过程：

第407场周赛t4，没做出来

- 差分数组

代码：

```cpp
class Solution {
public:
    long long minimumOperations(vector<int>& nums, vector<int>& target) {
        long long s = target[0] - nums[0];
        long long ans = abs(s);
        int n = nums.size();
        for (int i = 1; i < n; ++i) {
            long long k = (target[i] - target[i-1]) - (nums[i] - nums[i-1]);
            if (k > 0) {
                ans += s >= 0 ? k : max(k + s, 0LL);
            } else {
                ans -= s <= 0 ? k : min(k + s, 0LL);
            }
            s += k;
        } 
        return ans;
    }
};
```