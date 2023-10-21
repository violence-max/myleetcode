题目描述：

![image](/basical/array/image/image113.png)

解决过程：

第114场双周赛t3，没做出来

- and运算性质

代码：

```cpp
class Solution {
public:
    int maxSubarrays(vector<int>& nums) {
        int op = -1, n = nums.size();
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            op &= nums[i];
            if (op == 0) {
                ans++;
                op = -1;
            }
        }
        return ans == 0 ? 1 : ans;
    }
};
```