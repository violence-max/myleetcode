题目描述：

![image](/basical/simulation/image/image10.png)

解决过程：

第109场双周赛t1，ac

代码：

```cpp
class Solution {
public:
    bool isGood(vector<int>& nums) {
        int n = nums.size();
        int x = *max_element(nums.begin(), nums.end());
        if (x != n - 1) return false;
        sort(nums.begin(), nums.end());
        for (int i = 1; i < n - 1; ++i) {
            if (nums[i] != nums[i-1] + 1) return false;
        }
        return true;
    }
};
```