题目描述：

![image](/basical/array/image/image86.png)

解题过程：

第352场周赛t1，7分钟解决，wa两次

代码：（(O(n))）

```cpp
class Solution {
public:
    int longestAlternatingSubarray(vector<int>& nums, int threshold) {
        int res = INT_MIN;
        int i = 0;
        int n = nums.size();
        while (i < n) {
            if (nums[i] % 2 == 0 && nums[i] <= threshold) {
                int len = 1;
                int j = i + 1;
                for (; j < n; ++j) {
                    if (nums[j] % 2 != nums[j - 1] % 2 && nums[j] <= threshold) {
                        len++;
                    } else {
                        break;
                    }
                }
                res = max(res, len);
                i = j;
            } else {
                ++i;
            }
        }
        return res == INT_MIN ? 0 : res;
    }
};
```