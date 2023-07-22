题目描述：

![image](/basical/array/image/image96.png)

解决过程：

第108场双周赛t1，11分钟

代码：

```cpp
class Solution {
public:
    int alternatingSubarray(vector<int>& nums) {
        int ans = -1;
        int n = nums.size();
        int i = 0;
        while (i < n - 1) {
            if (nums[i + 1] - nums[i] == 1) {
                int j = i + 1;
                int temp = 1;
                while (j < n) {
                    if (nums[j] - nums[j-1] == temp) {
                        ++j;
                        temp *= -1;
                    } else {
                        break;
                    }
                }
                // cout << j << endl;
                ans = max(ans, j - i);
            }
            ++i;
        }
        return ans;
    }
};
```