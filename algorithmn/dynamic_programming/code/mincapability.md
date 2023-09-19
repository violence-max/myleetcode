题目描述：

![image](/algorithmn/dynamic_programming/image/image94.png)

解决过程：

没做出来

- 二分+枚举答案

代码：

```cpp
class Solution {
public:
    int minCapability(vector<int>& nums, int k) {
        int lower = *min_element(nums.begin(), nums.end());
        int upper = *max_element(nums.begin(), nums.end());
        while (lower <= upper) {
            int mid = (lower + upper) >> 1;
            int count = 0;
            bool vis = false;
            for (int x : nums) {
                if (x <= mid && !vis) {
                    count++;
                    vis = true;
                } else {
                    vis = false;
                }
            }
            if (count >= k) {
                upper = mid - 1;
            } else {
                lower = mid + 1;
            }
        }
        return lower;
    }
};
```