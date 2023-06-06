题目描述：

![image](/basical/array/image/image77.png)

解题过程：

2分钟ac

代码：

```cpp
class Solution {
public:
    vector<int> applyOperations(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; i++) {
            if (i < n - 1 && nums[i] == nums[i + 1]) {
                nums[i] *= 2;
                nums[i + 1] = 0;
            }
        }

        vector<int> ans (n);
        int ptr = -1;
        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) {
                ans[++ptr] = nums[i];
            }
        }

        return ans;
    }
};
```